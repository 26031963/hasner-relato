# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 16:39** · publicado em **12/11/2026 16:40** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 16:39 · sessao a061d30af7be · - · - · origem=Ronald (tmux)

Recenso na árvore após a cura, com `ast.parse` e inspeção dos operandos e consumidores, sem editar arquivos.

| Sítio | Classe | Interpretação |
|---|---|---|
| `relatorios/pdf_espelho.py:698` e `:702` | **IDENTIDADE — residual** | O conjunto `DD/MM HH:MM` alimenta substring que elimina anomalia. Ainda perde segundos. |
| `escala/utils.py:1760` | EXIBIÇÃO/projeção | Chave `(tipo, HH:MM do marco previsto)` localiza coluna da régua. Não compara a hora da batida. |
| `escala/utils.py:1772` | EXIBIÇÃO/projeção com promoção | A ausência do **marco previsto** na régua promove a célula correspondente; hora e instante são transportados juntos em `:1775-1776`. Não identifica a órfã pela sua hora. |
| `escala/utils.py:1778` | EXIBIÇÃO/projeção com promoção | Marco interno fora da régua ocupa slot `I`; se faltam slots, a célula inteira vira órfã, conservando o instante em `:1787-1788`. |
| `escala/utils.py:1797`, `:1800` | EXIBIÇÃO/projeção | Chave prevista seleciona e deduplica colunas das pontas. |
| `escala/utils.py:1801` | EXIBIÇÃO/projeção | `.index(_key4)` retorna a coluna do marco previsto. |
| `escala/utils.py:1809-1810` | EXIBIÇÃO/projeção | `in` e `.index` mapeiam cada marco previsto à coluna. A pertença da **batida** ao turno é decidida depois por instante em `:2104`. |
| `escala/utils.py:1897` | ORDENAÇÃO | Ordena pares hora/instante pela hora de exibição; conserva os pares em `:1898-1899`. |
| `escala/utils.py:626`, `:669` | EXIBIÇÃO | HH:MM serializado em paralelo com os instantes em `:628`, `:671`; identificação usa índices e datetimes. |
| `escala/services/leitor_celula.py:287`, `:302-303` | EXIBIÇÃO/projeção | Chaves de marcos previstos remapeiam células na régua. |
| `escala/services/leitor_celula.py:347-350` | EXIBIÇÃO/projeção | Deduplicação de colunas previstas por `(tipo, HH:MM)`. |
| `escala/services/leitor_celula.py:25`, `:30-35`, `:121` | EXIBIÇÃO/status | Truncamento para calcular desvio da luz em relação ao previsto; não decide identidade ou pertença de órfã. |
| `relatorios/pdf_espelho.py:714-715`, `:863`, `:867`, `:889`, `:939`, `:1030`, `:1077` | EXIBIÇÃO | Formata escala, batidas, procedência e emissão. Os filtros usam `b.orfa`, decidido em `:590`. |

Verificação dos sítios de identidade após a cura:

- Dry-run compara `datetime` em `escala/utils.py:1702`, `:1726`.
- Localizador da órfã compara ISO reconstruído com instante em `escala/utils.py:2068-2069`.
- Guarda usa timestamps do turno em `escala/utils.py:2092`; interseção por instante em `:2093-2095`; instante da célula em `:2100-2102`; unicidade em `:2103`; pertença e ordem em `:2104`.
- Leitor mantém pareamento posicional em `escala/services/leitor_celula.py:191-193`.
- PDF identifica órfãs e períodos por timestamp em `relatorios/pdf_espelho.py:195`, `:500`, `:590`, `:944-947`.

O achado de identidade reduzida remanescente neste escopo é o filtro textual do cabeçalho do PDF (`relatorios/pdf_espelho.py:698-702`).

## 12/11 16:36 · sessao a061d30af7be · - · - · origem=Ronald (tmux)

Censo preliminar feito com `ast.parse`, sem editar arquivos.

| Achado | Classe | Razão |
|---|---|---|
| `utils.py:2093,2097,2099` | IDENTIDADE | A redução para HH:MM decide pertença/unicidade de órfã e pertença da célula ao turno. |
| `pdf_espelho.py:698-702` | IDENTIDADE | Conjunto e substring sem segundos eliminam anomalias de outra batida no mesmo minuto. |
| `utils.py:1760` | EXIBIÇÃO/projeção | Chave `(tipo, HH:MM previsto)` alinha célula na coluna do marco. |
| `utils.py:1772,1778` | EXIBIÇÃO/projeção | Compara o marco previsto com a régua; quando promove uma célula, conserva seu instante (`utils.py:1776,1788`). |
| `utils.py:1797-1801` | EXIBIÇÃO/projeção | Identifica e deduplica colunas de marcos previstos; não compara órfãs/batidas. |
| `utils.py:1809-1810` | EXIBIÇÃO/projeção | `.index` localiza coluna do marco previsto. |
| `utils.py:1897` | ORDENAÇÃO | Ordena pares hora/instante, conservando o pareamento (`utils.py:1898-1899`). |
| `leitor_celula.py:287,302-303` | EXIBIÇÃO/projeção | Remapeia células por coluna prevista. |
| `leitor_celula.py:347-350` | EXIBIÇÃO/projeção | Deduplica colunas previstas da régua. |
| `leitor_celula.py:24-35,121` | EXIBIÇÃO/status | Calcula desvio, sem decidir identidade/orfandade. |
| `utils.py:626,669` | EXIBIÇÃO | Serializa HH:MM junto dos respectivos instantes (`utils.py:628,671`). |
| `pdf_espelho.py:863,867,939` | EXIBIÇÃO | Formata batidas cuja identidade já foi estabelecida por timestamp (`pdf_espelho.py:590`). |

Os caminhos já datados usam `datetime`: dry-run (`utils.py:1702,1726`), localizador da reconciliação (`utils.py:2068-2069`), órfãs publicadas/PDF (`pdf_espelho.py:195,500,590,944-947`) e pareamento posicional do leitor (`leitor_celula.py:191-193`). [nao verificado]

A célula de `_celula_do_marco` já carrega `instante.isoformat()` com offset (`utils.py:327-328`).

Aguardo a cura para repetir o censo na árvore final.

## 12/11 16:09 · sessao 7d019a0204b9 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**1. NAO PROCEDE a cura dos cinco achados como classe.** Quatro correções estão presentes; a reconciliação corrigiu a localização e a remoção, mas conservou outra decisão por HH:MM. (`app/escala/utils.py:2068`; `app/escala/utils.py:2097`)

| Achado | Situação após c19 |
|---|---|
| Reconciliação remove a órfã errada | Agora localiza pelo instante completo e remove a mesma posição nas duas listas. Porém, a guarda ainda associa órfãs e células ao turno por minuto. (`app/escala/utils.py:2068`; `app/escala/utils.py:2105`; `app/escala/utils.py:2097`; `app/escala/utils.py:2099`) |
| Dry-run usa conjunto de horas | Corrigido para conjunto de `datetime`. (`app/escala/utils.py:1702`; `app/escala/utils.py:1726`) |
| Contador sem chamador | Apagado, com lápide. (`app/ponto/services/cartorio.py:721`) |
| Leitor desalinha hora e instante | Filtra ambos pelo mesmo par posicional; lastro ausente/desalinhado permanece vazio. (`app/escala/services/leitor_celula.py:191`; `app/escala/services/leitor_celula.py:193`) |
| Adaptador perde órfãs sem marcos | Agora retorna as horas mesmo com `marcos=[]`. (`app/escala/utils.py:1105`; `app/escala/utils.py:1107`) |

**A guarda [nome]/ATA-X-TURNO decide pertença, não somente ordem:** `_hh_turno` reduz as batidas do turno a minutos; o filtro de `_orf_t` usa esses minutos para decidir se a entrada é a única órfã daquele turno; a célula posterior também é associada ao turno por sua hora. Somente `_ck['hora'] > _hhmm_t` é ordenação. (`app/escala/utils.py:2093`; `app/escala/utils.py:2097`; `app/escala/utils.py:2099`)

Contraexemplo no laço real em memória: turno não noturno `13:41 → 17:14:20`, marcos `11:00/16:00`, célula posterior ocupada por `17:14:20`; órfãs `13:41` e uma batida alheia ao turno às `17:14:10`. A entrada fica missing; deslocar somente a órfã alheia para `17:15:10` permite preencher a entrada. A decisão muda pela colisão de minuto; reprodução integral desse universo em produção **[nao verificado]**. (`app/escala/utils.py:2097`; `app/escala/utils.py:2101`; `app/escala/utils.py:2107`)

Nos outros três arquivos, não encontrei identidade de órfã **somente por HH:MM**: leitor e espelho transportam; o casamento central do PDF compara instantes. O filtro textual do PDF conserva data, mas perde segundos — tratado no item 6. (`app/escala/services/leitor_celula.py:193`; `app/ponto/services/espelho.py:860`; `app/relatorios/pdf_espelho.py:195`; `app/relatorios/pdf_espelho.py:698`)

**2. PROCEDE: o item 5 alcança um leitor que toma decisões e pode mudar número no PDF.** Há dois chamadores de produção: espelho e calendário. O calendário descarta o segundo retorno e já lê diretamente `dia['orfas']`; portanto, esse chamador não ganha órfãs pelo novo retorno. (`app/ponto/services/espelho.py:490`; `app/ponto/services/espelho.py:704`; `app/colaboradores/services/calendario.py:377`; `app/colaboradores/services/calendario.py:380`)

- **Tela do espelho:** publica as horas em `orfas_grade`, mas não passa automaticamente a desenhá-las em roxo quando `realizado_grade=[]`: os dois desenhos de órfãs estão dentro do ramo de realizado não vazio; o ramo alternativo mostra batidas cruas. (`app/ponto/services/espelho.py:859`; `app/templates/ponto/espelho.html:151`; `app/templates/ponto/espelho.html:231`; `app/templates/ponto/espelho.html:240`; `app/templates/ponto/espelho.html:243`)
- **PDF com lastro válido:** passa a reconhecer a batida como órfã, desenhá-la em cinza com asterisco e excluir os alertas do período que ela abriu. (`app/relatorios/pdf_espelho.py:494`; `app/relatorios/pdf_espelho.py:590`; `app/relatorios/pdf_espelho.py:858`; `app/relatorios/pdf_espelho.py:947`)
- **PDF sem lastro:** passa a declarar “Orfas: nao sei (ata sem instantes)”. (`app/relatorios/pdf_espelho.py:180`; `app/relatorios/pdf_espelho.py:587`; `app/relatorios/pdf_espelho.py:899`)
- **Número informativo:** uma entrada agora reconhecida como órfã deixa de contar em `turnos_abertos`; o dia também pode sair de `datas_turno_aberto`. A marca ainda alimenta a supressão de anomalias do cabeçalho. (`app/relatorios/pdf_espelho.py:537`; `app/relatorios/pdf_espelho.py:598`; `app/relatorios/pdf_espelho.py:702`)

O adaptador não grava ata. Não encontrei caminho do **segundo retorno alterado** até emissão de cobrança, chamado, pergunta ou cálculo financeiro: o calendário o descarta e o espelho o publica para seus consumidores. Os leitores diretos da ata continuam recebendo a lista anterior. (`app/escala/utils.py:1105`; `app/escala/utils.py:1132`; `app/colaboradores/services/calendario.py:377`; `app/ponto/services/espelho.py:859`; `app/chamados/juizes.py:510`)

Folga com batida, dia sem escala e isento só são alcançados por essa alteração quando chegam ao adaptador com órfãs e **marcos recebidos vazios**. Sem escala que satisfaça o gate do espelho, o adaptador nem é instalado; isenção não apaga as órfãs existentes. Quantidade de casos reais atingidos e impacto monetário **[nao verificado]**. (`app/escala/utils.py:1103`; `app/escala/utils.py:1107`; `app/ponto/services/espelho.py:488`; `app/ponto/services/espelho.py:707`; `app/escala/services/leitor_celula.py:214`)

**3. PROCEDE a cura do dry-run.** O conjunto recebe a serialização dos próprios instantes locais de `locs`, sem truncamento de segundos ou microssegundos. `fromisoformat` reconstrói o valor comparado; a identidade do objeto `tzinfo` não é requisito para essa comparação. (`app/escala/utils.py:1370`; `app/escala/utils.py:671`; `app/escala/utils.py:1702`; `app/escala/utils.py:1726`)

Não há saída normal de `_match_marcos` que devolva horas órfãs sem preencher o coletor fornecido: tanto a saída antecipada quanto a saída DP preenchem horas e instantes antes do retorno. No dry-run, `derivar=False`, sem `turno`, evita a derivação de papéis e segue pela DP; não esvazia o coletor por si. (`app/escala/utils.py:601`; `app/escala/utils.py:626`; `app/escala/utils.py:628`; `app/escala/utils.py:631`; `app/escala/utils.py:669`; `app/escala/utils.py:671`; `app/escala/utils.py:674`)

**4. NAO PROCEDE a igualdade universal de tamanho em todo escritor; o pareamento existe na cadeia com lastro completo.** (`app/escala/services/leitor_celula.py:191`; `app/ponto/services/espelho.py:503`)

| Dado | Quem escreve | Quem lê |
|---|---|---|
| `ata['orfas']` | Conteúdo por `ata_do_dia`; persistência pela porta. (`app/ponto/services/cartorio.py:739`; `app/ponto/portas/celula.py:995`; `app/ponto/portas/celula.py:1033`) | Leitor da célula, juiz de batida, cluster, vigia, esmeril, juiz de chamados, materialização, tripwire e helper do cartão. (`app/escala/services/leitor_celula.py:187`; `app/ponto/juiz_batida.py:161`; `app/ponto/services/cluster_espurio.py:31`; `app/ponto/services/vigia_pareador_celula.py:74`; `app/ponto/services/esmeril_espelho.py:125`; `app/chamados/juizes.py:510`; `app/chamados/services/materializacao.py:180`; `app/ponto/management/commands/tripwire_celulas.py:264`; `app/relatorios/cartao_pela_celula.py:288`) |
| `ata['orfas_instantes']` | Conteúdo por `ata_do_dia`; porta transporta a ata inteira. (`app/ponto/services/cartorio.py:742`; `app/ponto/portas/celula.py:995`) | `_orfas_da_ata`; o contador anterior foi retirado. (`app/escala/services/leitor_celula.py:190`; `app/ponto/services/cartorio.py:721`) |
| `dia['orfas_instantes']`, builder | `_match_marcos`, builders, promoção de células, conservação de sobras, overlays e reconciliação. (`app/escala/utils.py:628`; `app/escala/utils.py:671`; `app/escala/utils.py:799`; `app/escala/utils.py:1776`; `app/escala/utils.py:1788`; `app/escala/utils.py:1868`; `app/escala/utils.py:1890`; `app/escala/utils.py:2105`) | Sort pareado, reconciliação e cartório. (`app/escala/utils.py:815`; `app/escala/utils.py:1897`; `app/escala/utils.py:2068`; `app/ponto/services/cartorio.py:742`) |
| `dia['orfas_instantes']`, transportado | Leitor da ata; degradação do espelho esvazia deliberadamente. (`app/escala/services/leitor_celula.py:193`; `app/escala/services/leitor_celula.py:428`; `app/ponto/services/espelho.py:503`) | Espelho publica `orfas_instantes_grade`; PDF consome. (`app/ponto/services/espelho.py:860`; `app/relatorios/pdf_espelho.py:177`) |
| Retorno de `montar_realizado_grade` | O próprio adaptador copia as horas, conservando sua ordem. (`app/escala/utils.py:1105`; `app/escala/utils.py:1132`) | Espelho e calendário, conforme item 2. (`app/ponto/services/espelho.py:704`; `app/colaboradores/services/calendario.py:377`) |

Nos builders, append, promoção e remoção acompanham os dois membros; o sort é por HH:MM, conservando pares, e os overlays limpam ambos. (`app/escala/utils.py:810`; `app/escala/utils.py:815`; `app/escala/utils.py:977`; `app/escala/utils.py:1175`; `app/escala/utils.py:1254`; `app/escala/utils.py:1298`; `app/escala/utils.py:1304`; `app/escala/utils.py:1775`; `app/escala/utils.py:1787`; `app/escala/utils.py:2105`)

As exceções são explícitas: ata antiga/desalinhada e degradação podem ter horas com `instantes=[]`; há ata parcial somente em memória, sem instantes. Além disso, `ata_do_dia` copia as listas separadamente e não valida tamanho/conteúdo de uma entrada arbitrária. (`app/escala/services/leitor_celula.py:191`; `app/escala/services/leitor_celula.py:195`; `app/ponto/services/espelho.py:503`; `app/escala/utils.py:1948`; `app/escala/utils.py:1957`; `app/ponto/services/cartorio.py:739`; `app/ponto/services/cartorio.py:742`)

**5. NAO PROCEDE a suficiência dos testes para certificar a classe inteira.** Reproduzidos em memória: c18 **13**, c19 **9**, identidade datada **7** verdes; **14 mutantes mortos**, por falhas de asserção. São recortes do código, sem runner Django nem certificação dos bytes PDF. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:1`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:336`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:217`; `app/relatorios/tests/test_pdf_identidade_datada.py:6`)

O mutante `reconciliacao_minuto` morde o retorno ao HH:MM **no localizador `_ix_orfa`**. Entretanto, os testes fornecem marco igual ao instante alvo e `_abs_t=[alvo]`: a distância é zero, `_mais_perto=[]`, e a guarda residual nunca executa. Portanto, não certificam sua pertença/unicidade por instante. (`app/ponto/tests/test_lastro_c19_orfas_instantes.py:25`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:53`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:59`; `app/escala/utils.py:2089`; `app/escala/utils.py:2092`)

Persistem identidade reduzida na reconciliação e decisão própria de supressão no PDF. Não identifiquei no delta novo escritor fora da porta, leitura nova de relógio ou aquisição nova de trava; concorrência em execução **[nao verificado]**. (`app/escala/utils.py:2097`; `app/relatorios/pdf_espelho.py:702`; `app/ponto/services/cartorio.py:742`; `app/escala/services/leitor_celula.py:193`; `app/escala/utils.py:1700`)

**6. NAO PROCEDE integralmente a triagem como quitação da classe.** Procedem os cinco encaminhamentos e a remoção do contador pela L-111; o item 1, contudo, ficou incompleto pela guarda descrita acima. A prova de frota, reversão, relavratura da competência 11 e DIFF monetário zero **[nao verificado]**. (`app/ponto/services/cartorio.py:721`; `CLAUDE.md:191`; `app/escala/utils.py:2097`)

Quanto a `_espurias_datadas`, procede que ele conserva dia/mês. Não procede tratá-lo como simples formatação: ele elimina anomalias por substring de `DD/MM HH:MM`; a própria origem da string já perdeu segundos. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`; `app/ponto/adaptador_motor.py:41`)

Contraexemplo do filtro real: órfã `10/11 03:00:10` e anomalia de outra batida `10/11 03:00:20` produzem a mesma chave textual, permitindo apagar a segunda anomalia. Ocorrência em produção **[nao verificado]**. O selo aceita expressamente identidades por data/minuto, portanto seu verde não resolve essa colisão. (`app/relatorios/pdf_espelho.py:702`; `app/ponto/adaptador_motor.py:42`; `app/relatorios/tests/test_pdf_identidade_datada.py:301`; `app/relatorios/tests/test_pdf_identidade_datada.py:302`)

**Procede remeter o descarte ao opus**, como determina a regra; isso não equivale a declarar o achado curado. O alcance adicional do aval `RESPOSTAS-1211-1230`, citado somente pela triagem fornecida, **[nao verificado]**. (`CLAUDE.md:375`; `CLAUDE.md:379`)

**Fecho: NAO LIBERA para suite + opus nesta volta: resta decisão de pertença de órfã por HH:MM na reconciliação, fora da cobertura dos REDs apresentados.** (`app/escala/utils.py:2097`; `app/escala/utils.py:2099`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:59`)

## 12/11 16:07 · sessao 76402e02c93d · FIO-E0-V4-E3-LEITORES · verificador · origem=Code

**1. NAO PROCEDE — os avais ainda não estão integralmente realizados.**

Permanecem pelo menos três respondedores nomeados no aval:

- Relatório: decide “aguarda colaborador/supervisão” pelo carimbo cru (`app/chamados/relatorios_disputa.py:21`).
- Cobrança: seleciona somente `respondida_em__isnull=True` (`app/chamados/services/cobranca.py:229`).
- Cartão: anuncia “Resposta enviada” e “Aguardando a supervisão” apenas pela presença do carimbo (`app/templates/chamados/partials/_card_atendimento.html:94`, `:99`, `:101`).

A catraca admite explicitamente pendências, fixa `TETO = 35` e verifica apenas que esse número não aumentou; portanto, não demonstra “respondedores vivos esperado 0”. Ela ainda exige encontrar cobrança e relatório no universo (`app/chamados/tests/test_e3_catraca_respondedores.py:7`, `:16`, `:39`, `:44`).

**Quanto ao b4:** não identifiquei pressuposto remanescente do filtro transplantado nos sítios examinados. O painel exclui somente silêncio futuro; a lista do colaborador continua exigindo silêncio nulo; o teste conserva silêncio vencido pertencendo à fila, com verbo `historico` (`app/chamados/services/painel.py:190`; `app/chamados/services/janela_colab.py:73`; `app/chamados/tests/test_e3_portas_tarefa.py:95`). O “#4” restante em `test_e3_bloqueantes` trata do relógio do C6, não do silêncio vencido (`app/chamados/tests/test_e3_bloqueantes.py:3`).

Igualdade literal com MAIN e repetição da sonda C2: **[nao verificado]**.

**2. PROCEDE — a recusa está nos dois escritores apontados; não existe guarda universal de persistência.**

Ambos consultam a mesma porta, que chama `respondida_sem_texto` sobre o par solicitado; o predicado significa carimbo não nulo e texto vazio após `strip()` (`app/chamados/services/fio.py:1578`; `app/chamados/juizes.py:1510`, `:1518`).

| Escritor/caminho | Tratamento |
|---|---|
| `_nascer` | Recusa antes da trava e dos ramos criar/reusar/costurar (`app/chamados/models.py:2861`, `:2864`, `:2866`). |
| `responder_automaticamente` | Recusa antes das atribuições e do `save`; texto previamente existente mantém o no-op (`app/chamados/services/disputa_emissao.py:1647`, `:1655`, `:1657`). |
| `responder_pergunta_disputa_core` | Não chama a porta nova; rejeita texto vazio antes de escrever o par (`app/chamados/services/disputa_emissao.py:1432`, `:1467`, `:1508`). |
| `carimbar_resposta_pela_trilha` | Não chama a porta nova; exige texto existente antes de carimbar (`app/chamados/services/disputa_emissao.py:1675`, `:1679`). |
| `PerguntaDisputa.carimbar` | Só acrescenta carimbo quando `texto_vazio` é falso (`app/chamados/models.py:3155`, `:3157`, `:3177`). |
| `reabrir_para_correcao` | Limpa carimbo e texto por `update` ou `descarimbar_em_lote` (`app/chamados/services/disputa_emissao.py:1593`, `:1630`, `:1632`). |
| `ressuscitar_perguntas` | Limpa carimbo pelo lote (`app/chamados/models.py:1567`, `:3289`). |
| `_desfazer_em_memoria` | Restaura o carimbo anterior somente na instância (`app/chamados/models.py:3184`, `:3188`). |

**Existe escape estrutural:** `PerguntaDisputa.save` não valida esse par, e `descarimbar_em_lote(**outros)` aceita campos adicionais sem essa guarda (`app/chamados/models.py:3438`, `:3445`, `:3277`, `:3291`). A fixture demonstra criação crua do estado ilegal por `objects.create` (`app/chamados/tests/test_e3_10_cura_no_escritor.py:39`).

Outro escritor produtivo que efetivamente gere carimbo sem texto por esses escapes, inclusive admin/comando/signal: **[nao verificado]**. Os outros escritores identificados acima têm guardas próprias; não seria correto apresentá-los como novas fontes comprovadas do estado ilegal (`app/chamados/services/disputa_emissao.py:1432`, `:1675`; `app/chamados/models.py:3155`).

**3. NAO PROCEDE — os leitores ainda não compartilham inteiramente o significado.**

| Campo/valor | Quem produz | Quem lê |
|---|---|---|
| Retorno de `tarefa_do_dia` | Corpo da tarefa e publicação de portas, em memória (`app/chamados/services/tarefa_do_dia.py:184`, `:525`, `:540`). | Folha, vigia, beco, pauta e instrumento de sombra (`app/folha/fora_da_folha.py:74`; `app/chamados/services/vigia_fio.py:638`; `app/escala/services/cadastro_realidade.py:497`; `app/ponto/services/pautas_dos_discordantes.py:80`; `app/chamados/management/commands/diff_e3_sombra.py:128`). |
| `respondida_em` / `resposta_colab` | Escritores censados na pergunta 2 (`app/chamados/services/disputa_emissao.py:1467`, `:1657`; `app/chamados/models.py:3157`). | Crédito admin, C7, aviso do fio, API, pendências, relatório, cobrança e template (`app/chamados/services/raiox.py:143`; `app/chamados/models.py:1849`; `app/chamados/services/fio.py:174`; `app/api/views.py:1175`; `app/chamados/services/pergunta_no_app.py:71`; `app/chamados/relatorios_disputa.py:21`; `app/chamados/services/cobranca.py:229`; `app/templates/chamados/partials/_card_atendimento.html:94`). |
| `respostas_sem_veredito` | C7 seleciona não validada, não materializada, com resposta e fora de `respondida_sem_texto`, respeitando tranca (`app/chamados/models.py:1847`). | `pode_fechar` e porta de fechamento (`app/chamados/models.py:1860`, `:1872`). |
| Motivo/razão `respondida_sem_texto` | A exceção usa o par solicitado; a tarefa usa o texto da última decisão RESPOSTA (`app/chamados/models.py:2325`; `app/chamados/services/fio.py:1585`; `app/chamados/services/tarefa_do_dia.py:135`). | C7, aviso do cartão e diagnóstico do vigia (`app/chamados/models.py:1851`; `app/chamados/services/fio.py:174`; `app/chamados/services/vigia_fio.py:679`). |
| Papel do vigia | Vem de `fio`, cuja precedência é colaborador → admin → sistema (`app/chamados/services/tarefa_do_dia.py:552`). | Vigia repassa `fio` e conta exigência de admin por igualdade com `'admin'` (`app/chamados/services/vigia_fio.py:650`). |
| `fio_vivo` | Redução `any(pergunta_viva)` no universo recebido (`app/chamados/juizes.py:1494`, `:1503`). | C7 e aviso do colaborador (`app/chamados/models.py:1860`; `app/chamados/services/fio.py:174`). |
| `pode_fechar` | C7 exige simultaneamente lista de respostas sem veredito vazia e ausência de fio vivo (`app/chamados/models.py:1860`). | Explicador, agora alimentado com a disputa inteira (`app/chamados/services/explicador.py:139`; `app/chamados/services/fio.py:333`). |

**Tempo:** o dia de entrada é o dia do turno; `data_civil` vem do instante local do marco. A janela é inclusiva, e decisões/passivo/efeitos aceitam igualdade com `agora` (`app/chamados/services/tarefa_do_dia.py:105`, `:184`, `:519`, `:393`, `:401`, `:412`). O retorno não representa reconstrução histórica integral: identidade, cadastro, fila/status e tranca usam estado atual, conforme contrato declarado (`app/chamados/services/tarefa_do_dia.py:352`).

**Divergência concreta:** carimbo com texto vazio vira `cobrar` no crédito admin, mas continua fora das pendências do app, fora dos calados e classificado como aguardando supervisão no relatório (`app/chamados/services/raiox.py:144`; `app/chamados/services/pergunta_no_app.py:71`; `app/chamados/services/cobranca.py:229`; `app/chamados/relatorios_disputa.py:21`).

**4. PROCEDE — há classes de reprovação presentes.**

- **Derivação paralela remanescente:** o relatório declara `PRONTA_FECHAR` quando não há perguntas vivas, sem consultar a outra metade do C7, respostas sem veredito (`app/chamados/relatorios_disputa.py:18`; `app/chamados/models.py:1860`). O próprio teste novo do explicador demonstra cenário com C6 morto e C7 recusando fechamento (`app/chamados/tests/test_explicador_disputa.py:150`).
- **Relógio fora da borda:** `censo_alcance` exige `agora`, mas, omitido `hoje`, lê `timezone.localdate()`. Competência e teto usam esse dia, enquanto a tarefa usa `agora`; uma consulta histórica pode misturar relógios (`app/chamados/services/vigia_fio.py:542`, `:567`, `:584`, `:637`, `:653`). `furo_sem_canal.medir` também mantém fallback interno a `timezone.now()` (`app/chamados/services/furo_sem_canal.py:43`, `:56`).
- **Selo insuficiente para o resultado exigido:** teto 35 não prova morte dos cinco respondedores, e o detector exclui tuplas e templates (`app/chamados/tests/test_e3_catraca_respondedores.py:7`, `:10`, `:16`, `:39`).

No trajeto corrigido do explicador, ele agora consulta o C7 registrado; a guarda nova do nascimento executa antes da trava (`app/chamados/services/explicador.py:139`; `app/chamados/models.py:2864`). Inversão de trava ou pouso antes do pré-requisito: **[nao verificado]**.

**5. PROCEDE — existe mutante plausível sem RED correspondente.**

Mutante: executar a guarda de `_nascer` somente sob `if conversa:`. Isso deixaria `abrir` factual sem recusa, preservando o caminho exercitado pelos novos testes, que chamam exclusivamente `abrir_conversa` (`app/chamados/models.py:2845`, `:2864`; `app/chamados/tests/test_e3_10_cura_no_escritor.py:85`, `:98`, `:105`).

Deveria morder um RED por `PerguntaDisputa.abrir`, com dia válido, carimbo e texto vazio, incluindo nascimento e reuso/costura, exigindo motivo `respondida_sem_texto` e nenhuma alteração — o contrato promete recusa em todos esses ramos (`app/chamados/models.py:2861`). Sobrevivência executada contra todos os testes do diff: **[nao verificado]**.

**6. NAO SEI — não é possível provar invariância individual do passivo com o raspado fornecido.**

Há mudanças visíveis: API e web alteram a possibilidade de solicitar correção; a fila admin reclassifica carimbo sem texto para cobrança do colaborador (`app/api/views.py:1175`; `app/chamados/services/fio.py:361`; `app/chamados/services/raiox.py:143`). Os testes exigem expressamente essas mudanças (`app/chamados/tests/test_e3_leitores_fio_disputa.py:156`; `app/chamados/tests/test_creditos_perguntas_invariantes.py:94`).

Esses caminhos estão identificados no código como item 12 e cura do crédito; sua existência não demonstra um apply de limpeza do passivo (`app/chamados/services/fio.py:164`; `app/chamados/services/raiox.py:139`). A lista de pendências continua usando o carimbo cru, e o badge conta essa lista (`app/chamados/services/pergunta_no_app.py:71`, `:91`).

Provar que nenhuma vista dos 142/16 muda, ou reconciliar esse recorte com os 143/17 citados no aval: **[nao verificado]**.

**Fecho: NAO LIBERA para suíte + opus.** O bloqueio verificável é a entrega incompleta dos respondedores e do selo exigido, além do relógio ainda independente no vigia (`app/chamados/relatorios_disputa.py:21`; `app/chamados/services/cobranca.py:229`; `app/templates/chamados/partials/_card_atendimento.html:94`; `app/chamados/tests/test_e3_catraca_respondedores.py:39`; `app/chamados/services/vigia_fio.py:567`).

## 12/11 16:07 · sessao 95b777412492 · - · - · origem=Ronald (tmux)

1. **NAO PROCEDE a cura integral como classe.** O localizador e a remoção da órfã passaram a usar o instante completo e a mesma posição nas duas listas (`app/escala/utils.py:2068`; `app/escala/utils.py:2069`; `app/escala/utils.py:2105`; `app/escala/utils.py:2106`). Mas a guarda [nome]/ATA-X-TURNO ainda:
   - constrói um conjunto dos minutos das batidas do turno (`app/escala/utils.py:2093`);
   - identifica quais órfãs pertencem ao turno por pertença nesse conjunto, decidindo se existe uma única órfã do turno (`app/escala/utils.py:2097`);
   - identifica se a célula real pertence ao turno por minuto (`app/escala/utils.py:2099`).

   Portanto, não é apenas ordem: somente `_ck['hora'] > _hhmm_t` é comparação de ordem. As duas pertenças anteriores decidem identidade/associação (`app/escala/utils.py:2097`; `app/escala/utils.py:2099`).

2. **Contraexemplo executado no laço real, sem arquivo:** turno não noturno com entrada `13:41` e saída `17:14:20`; marcos `11:00`/`16:00`; célula posterior com instante `17:14:20`; órfã alvo `13:41` e outra órfã, fora de `_t.batidas`, às `17:14:10`. A entrada permanece missing; mover exclusivamente a órfã alheia para `17:15:10` faz a mesma entrada ocupar o marco. Isso decorre do filtro por minuto citado; reprodução integral do caso pelo builder/banco **[nao verificado]** (`app/escala/utils.py:2093`; `app/escala/utils.py:2097`; `app/escala/utils.py:2101`; `app/escala/utils.py:2107`).

3. **PROCEDE a cura do dry-run.** `locs` recebe `timezone.localtime`; o coletor serializa os próprios objetos de `locs` com `isoformat`; o set os reconstrói por `fromisoformat` e compara `datetime`, preservando segundos/microssegundos (`app/escala/utils.py:1370`; `app/escala/utils.py:671`; `app/escala/utils.py:1702`; `app/escala/utils.py:1726`). Executei o `_match_marcos` real com `03:00:20.654321-03:00`: o coletor preservou `.654321`, e o round-trip comparou igual **[nao verificado: ensaio em memória sem log persistido]**.

   Não há saída normal retornando horas sem preencher o coletor fornecido: a saída antecipada escreve ambos antes de retornar; a saída DP também (`app/escala/utils.py:626`; `app/escala/utils.py:628`; `app/escala/utils.py:631`; `app/escala/utils.py:669`; `app/escala/utils.py:671`; `app/escala/utils.py:674`). `derivar=False` apenas evita a derivação de papéis, seguindo para a DP (`app/escala/utils.py:601`; `app/escala/utils.py:632`).

4. **Os REDs não mordem a guarda residual.** O helper c19 fornece marco alvo igual ao instante da batida e `_abs_t=[alvo]`; assim `_d_alvo=0` e `_mais_perto=[]`, sem entrar na guarda (`app/ponto/tests/test_lastro_c19_orfas_instantes.py:53`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:59`; `app/escala/utils.py:2089`; `app/escala/utils.py:2092`). O mutante oficial `reconciliacao_minuto` troca apenas `_ix_orfa`, cobrindo o localizador corrigido, não a guarda (`app/ponto/tests/test_lastro_c19_orfas_instantes.py:25`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:28`).

   Rodei os **29 testes c18+c19+identidade_datada** com dois mutantes adicionais em memória: desligar integralmente a guarda de `2092`; ou substituir seu corpo por `_mais_perto=[]`, liberando toda reconciliação nessa situação. **Ambos sobreviveram: 29 testes verdes em cada execução** **[nao verificado: saída observada em memória, sem log persistido]**. O sítio de produção afetado é `app/escala/utils.py:2092`.

5. No PDF, o casamento central usa instantes completos e recusa ISO sem offset; o filtro `_espurias_datadas` continua reduzindo para `DD/MM HH:MM` e suprimindo anomalia por substring (`app/relatorios/pdf_espelho.py:183`; `app/relatorios/pdf_espelho.py:186`; `app/relatorios/pdf_espelho.py:195`; `app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`). Ele tem data, mas permanece decisão com identidade reduzida; o descarte por escopo depende do aval literal fornecido pelo usuário **[nao verificado além dos anexos]**.

Há também comentário desatualizado afirmando que o casamento da reconciliação é por HH:MM, apesar do localizador novo (`app/escala/utils.py:1996`).

## 12/11 16:07 · sessao 6b8954ecf85b · - · - · origem=Ronald (tmux)

**Q5 — NAO PROCEDE a cura integral da classe.** Os testes mordem a regressão do lookup por minuto, mas não exercitam a guarda [nome]/ATA-X-TURNO: o helper entrega marco e batida no mesmo instante, com `_abs_t=[alvo]`; assim, `_mais_perto=[]` e o ramo residual por HH:MM não executa. (`app/ponto/tests/test_lastro_c19_orfas_instantes.py:53`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:59`; `app/escala/utils.py:2089`; `app/escala/utils.py:2092`)

**Ensaios reproduzidos:** c18 13 verdes, c19 9 verdes, identidade datada 7 verdes: 29 testes, todos rc 0; 9 mutantes c18 e 5 c19 mortos com falhas de asserção, nenhum dependendo de erro de infraestrutura. São recortes em memória, sem inicialização Django ou renderização dos bytes PDF. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:1`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:336`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:217`; `app/relatorios/tests/test_pdf_identidade_datada.py:1`)

O mutante `reconciliacao_minuto` reverte somente `_ix_orfa`; falhou em três subtests: instante ausente e mesmo minuto com offsets BRT/UTC. O mutante `dryrun_minuto` falhou nos dois testes, incluindo aquele que chama `_match_marcos` real. (`app/ponto/tests/test_lastro_c19_orfas_instantes.py:25`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:64`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:75`; `app/ponto/tests/test_lastro_c19_orfas_instantes.py:121`)

**Q6 — NAO PROCEDE chamar o descarte de cura ou ausência de decisão.** `_espurias_datadas` conserva dia/mês, mas decide supressão de anomalias por substring de minuto. O formato da anomalia também perdeu segundos na origem. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`; `app/ponto/adaptador_motor.py:39`)

Executei o filtro real em memória com órfã `10/11 03:00:10`, batida não órfã `10/11 03:00:20` e anomalia `Entrada sem saída em 10/11 03:00.`: a anomalia foi removida. Esse é contraexemplo da colisão textual; ocorrência desse universo pelo pareador ou em produção **[nao verificado]**. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`; `app/ponto/adaptador_motor.py:41`)

O selo atual aceita explicitamente `(data, HH:MM)` e `DD/MM HH:MM`; o RED S2 cobre datas/mês diferentes, não segundos diferentes no mesmo minuto. (`app/relatorios/tests/test_pdf_identidade_datada.py:247`; `app/relatorios/tests/test_pdf_identidade_datada.py:301`)

**Procede encaminhar o descarte ao opus**, pois essa é a regra expressa; não procede tratá-lo como certificação de que o leitor não decide identidade reduzida. O escopo do verificador inclui leitores dos valores tocados, mesmo fora do diff. (`CLAUDE.md:375`; `CLAUDE.md:379`)

A remoção do contador com lapide procede pela L-111; a sonda de frota/reversão/DIFF monetário zero continua **[nao verificado]**. (`app/ponto/services/cartorio.py:721`; `app/docs/LEIS.md:137`)

## 12/11 16:06 · sessao 4d31fddac01f · - · - · origem=Ronald (tmux)

Q2 — **PROCEDE, com efeito adicional em número informativo do PDF.**

- Há dois chamadores de produção: `montar_dias`, com alias `montar_realizado`, em `app/ponto/services/espelho.py:490`, `app/ponto/services/espelho.py:704`; e calendário, com alias `_montar_real_grade`, em `app/colaboradores/services/calendario.py:333`, `app/colaboradores/services/calendario.py:377`.
- No calendário, o segundo retorno é descartado; as órfãs já eram lidas diretamente do dia. Portanto esse chamador não ganha nova informação por causa do item 5. (`app/colaboradores/services/calendario.py:377`; `app/colaboradores/services/calendario.py:380`)
- No espelho, sem marcos continua `real=[]`, mas agora `_orfas_grade` conserva as horas. (`app/escala/utils.py:1105`; `app/escala/utils.py:1107`; `app/ponto/services/espelho.py:704`; `app/ponto/services/espelho.py:859`)
- **A TELA não passa automaticamente a desenhar órfãs roxas nesse caso:** tanto o cartão quanto o minicalendário só percorrem `orfas_grade` dentro do ramo `realizado_grade` verdadeiro; com `real=[]`, mostram as batidas cruas pelo ramo alternativo. (`app/templates/ponto/espelho.html:151`; `app/templates/ponto/espelho.html:231`; `app/templates/ponto/espelho.html:240`; `app/templates/ponto/espelho.html:243`)
- **O PDF passa a ver e decidir com essa órfã.** Com lastro válido, `_orfas_publicadas` devolve os timestamps, o coletor passa a marcar `b.orfa`, e a batida vira `*HH:MM` cinza. Sem lastro, passa a declarar “Orfas: nao sei (ata sem instantes)”. (`app/relatorios/pdf_espelho.py:174`; `app/relatorios/pdf_espelho.py:494`; `app/relatorios/pdf_espelho.py:587`; `app/relatorios/pdf_espelho.py:590`; `app/relatorios/pdf_espelho.py:858`; `app/relatorios/pdf_espelho.py:867`; `app/relatorios/pdf_espelho.py:899`)
- **Não se pode afirmar “nenhum número muda”:** se a entrada de período aberto agora for reconhecida como órfã, deixa de integrar `resumo['turnos_abertos']`; também deixa de integrar a lista `datas_turno_aberto`. (`app/relatorios/pdf_espelho.py:191`; `app/relatorios/pdf_espelho.py:535`; `app/relatorios/pdf_espelho.py:537`; `app/relatorios/pdf_espelho.py:598`)
- A marca também entra no filtro de anomalias do cabeçalho. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`)
- A função só copia e retorna listas: não escreve a ata. Os consumidores financeiros do espelho leem realizado/rubricas por outras fontes; o segundo retorno é somente publicado em `orfas_grade`. (`app/escala/utils.py:1097`; `app/escala/utils.py:1132`; `app/ponto/services/espelho.py:798`; `app/ponto/services/espelho.py:826`; `app/ponto/services/espelho.py:859`)
- Não encontrei caminho do segundo retorno até emissão de cobrança/chamado/pergunta: o calendário descarta-o, e o espelho só o publica para desenho/PDF. Já os consumidores diretos de `dia['orfas']`/`ata['orfas']` continuam vendo a lista que já viam. (`app/colaboradores/services/calendario.py:377`; `app/ponto/services/espelho.py:704`; `app/ponto/services/espelho.py:859`)
- Folga/dia sem escala/isento só mudam por esse item se chegarem ao adaptador com órfãs e marcos vazios; a função não distingue esses tipos. Há gate de vínculo no espelho: se não satisfizer a condição de escala, o adaptador nem é instalado e a saída continua vazia. Isenção não limpa órfãs. (`app/escala/utils.py:1103`; `app/escala/utils.py:1107`; `app/ponto/services/espelho.py:488`; `app/ponto/services/espelho.py:707`; `app/escala/services/leitor_celula.py:214`; `app/escala/services/leitor_celula.py:223`)

Q4 — **PROCEDE para a cadeia normal; NÃO PROCEDE a afirmação universal de tamanho igual em todo escritor.**

| Dado | Escritores | Leitores diretos |
|---|---|---|
| `ata['orfas']` persistida | Produtor `ata_do_dia`; porta copia `_nova` e salva. (`app/ponto/services/cartorio.py:739`; `app/ponto/portas/celula.py:995`; `app/ponto/portas/celula.py:1006`; `app/ponto/portas/celula.py:1033`) | Leitor da célula (`app/escala/services/leitor_celula.py:187`); esmeril (`app/ponto/services/esmeril_espelho.py:125`); vigia pareador (`app/ponto/services/vigia_pareador_celula.py:74`); cluster (`app/ponto/services/cluster_espurio.py:29`); juiz batida (`app/ponto/juiz_batida.py:161`); juiz chamados (`app/chamados/juizes.py:510`); materialização (`app/chamados/services/materializacao.py:180`); tripwire (`app/ponto/management/commands/tripwire_celulas.py:264`); helper `orfas_e_isencao_da_ata` (`app/relatorios/cartao_pela_celula.py:288`). |
| `ata['orfas_instantes']` | Produtor `ata_do_dia`, cópia independente da lista recebida; porta transporta a ata inteira. (`app/ponto/services/cartorio.py:742`; `app/ponto/portas/celula.py:995`) | `_orfas_da_ata`. O contador foi apagado. (`app/escala/services/leitor_celula.py:190`; `app/ponto/services/cartorio.py:721`) |
| `dia['orfas_instantes']` builder | Coletor `_match_marcos` em ambos retornos; builders; conservação de sobras; promoção de célula para órfã; resets; reconciliação. (`app/escala/utils.py:628`; `app/escala/utils.py:671`; `app/escala/utils.py:799`; `app/escala/utils.py:811`; `app/escala/utils.py:1776`; `app/escala/utils.py:1788`; `app/escala/utils.py:1868`; `app/escala/utils.py:1890`; `app/escala/utils.py:1893`; `app/escala/utils.py:2105`) | Sort pareado; reconciliação; cartório. (`app/escala/utils.py:815`; `app/escala/utils.py:1897`; `app/escala/utils.py:2068`; `app/ponto/services/cartorio.py:742`) |
| `dia['orfas_instantes']` lido | Base vazia; leitor preenche junto com o filtro das horas. Degradação do espelho esvazia deliberadamente. (`app/escala/services/leitor_celula.py:395`; `app/escala/services/leitor_celula.py:193`; `app/escala/services/leitor_celula.py:428`; `app/ponto/services/espelho.py:503`) | Publicação como `orfas_instantes_grade`, depois `_orfas_publicadas`. (`app/ponto/services/espelho.py:860`; `app/relatorios/pdf_espelho.py:177`) |
| Retorno `montar_realizado_grade` | Somente o adaptador: cópia das horas na mesma ordem, `real=[]` quando não há marcos. (`app/escala/utils.py:1105`; `app/escala/utils.py:1107`; `app/escala/utils.py:1132`) | Os dois chamadores descritos acima. (`app/ponto/services/espelho.py:704`; `app/colaboradores/services/calendario.py:377`) |

Os escritores normais acrescentam e removem os dois membros juntos; o sort é **por HH:MM preservando pares**, não por instante absoluto. Os overlays limpam ambos; o legacy conserva o dicionário por `**sd`. (`app/escala/utils.py:810`; `app/escala/utils.py:815`; `app/escala/utils.py:977`; `app/escala/utils.py:1062`; `app/escala/utils.py:1175`; `app/escala/utils.py:1254`; `app/escala/utils.py:1298`; `app/escala/utils.py:1304`; `app/escala/utils.py:1775`; `app/escala/utils.py:1787`; `app/escala/utils.py:1897`; `app/escala/utils.py:2105`)

As exceções são explícitas: ata antiga/desalinhada produz horas e `instantes=[]`; degradação do builder conserva horas e apaga instantes; existe ata parcial **em memória** com apenas lâmpadas e órfãs, sem instantes. O cartório copia as duas listas sem validar comprimento ou conteúdo, portanto não impõe pareamento a uma entrada arbitrária. (`app/escala/services/leitor_celula.py:191`; `app/escala/services/leitor_celula.py:195`; `app/ponto/services/espelho.py:503`; `app/escala/utils.py:1948`; `app/escala/utils.py:1957`; `app/ponto/services/cartorio.py:739`; `app/ponto/services/cartorio.py:742`)

`carimbar_precedencia` pode persistir outra chave na ata, preservando as listas existentes; não é produtor do conteúdo novo. (`app/ponto/portas/celula.py:1083`; `app/ponto/portas/celula.py:1087`)

## 12/11 16:06 · sessao 9404cc491001 · - · - · origem=Ronald (tmux)

Achados para perguntas 3–5:

- **Relógio fora da borda presente:** `censo_alcance(..., *, agora)` ainda executa `hoje = hoje or timezone.localdate()`; `hoje` escolhe competências e teto civil enquanto a tarefa recebe `agora`. Omitir `hoje` com `agora` histórico mistura dois relógios (`app/chamados/services/vigia_fio.py:542`, `:567`, `:584`, `:637`, `:640`, `:653`). `furo_sem_canal.medir` também mantém fallback interno a `timezone.now()` (`app/chamados/services/furo_sem_canal.py:43`, `:56`).

- **Mutante plausível sobrevivente ao diff:** restringir a guarda nova de `_nascer` a `if conversa:` deixa `abrir` datada sem proteção; a guarda real atende ambos hoje, mas os testes novos de nascimento chamam exclusivamente `abrir_conversa` (`app/chamados/models.py:2796`, `:2809`, `:2845`, `:2864`; `app/chamados/tests/test_e3_10_cura_no_escritor.py:85`, `:98`, `:105`). Sobrevivência executada: **[nao verificado]**. Deve morder um RED chamando `PerguntaDisputa.abrir` com dia válido, `respondida_em=INSTANTE`, texto vazio, incluindo reuso/costura, exigindo motivo correto e nenhuma alteração (`app/chamados/models.py:2861`).

- **Catraca insuficiente para o aval:** teto é 35, teste exige somente `<=35`, e o próprio selo exige encontrar respondedores antigos; não demonstra “respondedores vivos esperado 0” (`app/chamados/tests/test_e3_catraca_respondedores.py:6`, `:16`, `:37`, `:44`). O detector não cobre tuplas nem templates, declarado nele (`app/chamados/tests/test_e3_catraca_respondedores.py:10`).

- **C7 corrigido sem juiz duplicado nesse trajeto:** `pode_fechar` compõe ausência de respostas sem veredito e ausência de fio vivo; explicador agora lê C7; fio fornece universo da disputa inteira, voltando à consulta integral para disputa fechada (`app/chamados/models.py:1847`, `:1854`; `app/chamados/services/explicador.py:139`; `app/chamados/services/fio.py:333`). `fio_vivo` significa somente existência de pergunta viva no universo fornecido pelo chamador, portanto não equivale a `pode_fechar` (`app/chamados/juizes.py:1494`).

Censo semântico:

| Valor | Escritor/autoridade | Leitores relevantes |
|---|---|---|
| Retorno `tarefa_do_dia` | Dict inicial e ações; projeção de portas dentro do mesmo juiz (`app/chamados/services/tarefa_do_dia.py:184`, `:243`, `:540`) | Porta é mera projeção; vigia, folha, beco e pauta leem-na (`app/chamados/services/tarefa_do_dia.py:559`; `app/chamados/services/vigia_fio.py:637`; `app/folha/fora_da_folha.py:67`; `app/escala/services/cadastro_realidade.py:499`; `app/ponto/services/pautas_dos_discordantes.py:80`) |
| Dia do turno / dia civil | Entrada `dia_do_turno`; civil do marco vem do teto convertido ao fuso local (`app/chamados/services/tarefa_do_dia.py:94`, `:105`, `:184`) | Janela da carga é inclusiva (`app/chamados/services/tarefa_do_dia.py:519`) |
| Instante | Decisões, passivo e efeitos aceitam igualdade `<= agora`; identidade, cadastro, fila/status e tranca continuam atuais por contrato explícito (`app/chamados/services/tarefa_do_dia.py:352`, `:393`, `:401`, `:412`) | Não se pode interpretar esse retorno como reconstrução histórica integral (`app/chamados/services/tarefa_do_dia.py:357`) |
| `fio` / papel do vigia | Precedência colab → admin → sistema no juiz (`app/chamados/services/tarefa_do_dia.py:552`) | Vigia repassa fio e conta admin por igualdade, sem traduzir verbo (`app/chamados/services/vigia_fio.py:645`) |
| `respondida_sem_texto` | Carimbo não nulo + `texto_vazio` (`app/chamados/juizes.py:1510`, `:1515`) | Guarda, C7 e aviso do cartão (`app/chamados/services/fio.py:1578`; `app/chamados/models.py:1851`; `app/chamados/services/fio.py:174`) |
| Razão `'respondida_sem_texto'` da tarefa | Última decisão RESPOSTA com `ato_texto` vazio (`app/chamados/services/tarefa_do_dia.py:135`) | É lida no retorno da tarefa; difere em insumo do predicado sobre projeção atual por desenho de sequência/instante (`app/chamados/services/tarefa_do_dia.py:118`, `:393`) |

O campo `data_evento` ainda possui dois significados declarados, separados por `conversa`: turno em `abrir`, civil em `abrir_conversa`; C19 exclui conversas da carga (`app/chamados/models.py:2803`, `:2814`; `app/chamados/services/tarefa_do_dia.py:375`). Não é mudança introduzida nos hunks do diff fornecido **[nao verificado por Git: cópia sem .git]**.

## 12/11 16:05 · sessao d2f5aa522f8b · - · - · origem=Ronald (tmux)

1. **NAO PROCEDE** para cumprimento integral: três respondedores nomeados seguem ativos — relatório decide pelo carimbo em `app/chamados/relatorios_disputa.py:21`, cobrança filtra carimbo nulo em `app/chamados/services/cobranca.py:229`, e template declara resposta enviada pelo carimbo em `app/templates/chamados/partials/_card_atendimento.html:94`. A catraca admite pendências, permite teto 35 e verifica apenas `<= TETO`: `app/chamados/tests/test_e3_catraca_respondedores.py:7`, `:16`, `:39`.

   **Não identifiquei pressuposto b4 nos sítios examinados.** O painel conserva exclusão somente de silêncio futuro (`app/chamados/services/painel.py:190`), seu contador idem (`:216`), e a lista colaborador continua exigindo silêncio nulo (`app/chamados/services/janela_colab.py:73`). O teste de silêncio vencido conserva a diferença antiga entre pertencimento à fila e verbo histórico (`app/chamados/tests/test_e3_portas_tarefa.py:95`, `:98`). Comparação literal com MAIN: **[nao verificado]**, pois a cópia não contém repositório Git.

6. **NAO SEI** quanto à invariância concreta dos 142/16: o raspado não fornece as vistas individuais **[nao verificado]**. O código muda a flag de correção da API (`app/api/views.py:1175`) e as flags web (`app/chamados/services/fio.py:361`); o teste exige expressamente flag falsa para pergunta carimbada sem texto (`app/chamados/tests/test_e3_leitores_fio_disputa.py:156`). A fila admin passa a atribuir esse estado ao colaborador (`app/chamados/services/raiox.py:143`, `:144`), com teste que exige `cobrar` (`app/chamados/tests/test_creditos_perguntas_invariantes.py:98`).

   Esses caminhos correspondem aos itens 12/#8 dos avais anexos **[nao verificado — anexo sem numeração]**; não demonstram aplicação sobre o passivo. A lista de pendências permanece separada pelo carimbo cru e continua classificando carimbada sem texto como histórico (`app/chamados/services/pergunta_no_app.py:71`, `:74`); seu badge conta essa lista (`:91`). Logo, a migração de leitores permanece incompleta (`app/chamados/tests/test_e3_catraca_respondedores.py:8`).

**Recomendação: NAO LIBERA**, pelo requisito verificavelmente aberto de eliminar os respondedores nomeados (`app/chamados/relatorios_disputa.py:21`; `app/chamados/services/cobranca.py:229`; `app/templates/chamados/partials/_card_atendimento.html:94`).

## 12/11 16:05 · sessao 57dfd31229d6 · - · - · origem=Ronald (tmux)

**Q2 — PROCEDE nos dois escritores curados; não há guarda universal no modelo.**

| Escritor | Tratamento do par |
|---|---|
| `_nascer` | Recusa antes da trava e antes dos ramos criar/reusar/costurar: `app/chamados/models.py:2863-2865`; inserção em `:3039-3042`. |
| `responder_automaticamente` | Recusa antes da atribuição e do save: `app/chamados/services/disputa_emissao.py:1651-1660`. Texto previamente existente produz no-op antes da guarda: `:1647-1648`, coberto em `app/chamados/tests/test_e3_10_cura_no_escritor.py:73-78`. |
| `responder_pergunta_disputa_core` | Escritor fora dessa porta, com guarda própria de texto não vazio: `app/chamados/services/disputa_emissao.py:1432-1434`; escreve ambos em `:1467-1468`, salva em `:1508`. |
| `carimbar_resposta_pela_trilha` | Escritor fora dessa porta, exige texto existente: `app/chamados/services/disputa_emissao.py:1675`; carimba/salva em `:1679-1681`. Comando consumidor: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67-68`. |
| `PerguntaDisputa.carimbar` | Escritor fora dessa porta; só põe `respondida_em` com `texto_vazio` falso: `app/chamados/models.py:3155-3158`; save em `:3177`. |
| `reabrir_para_correcao` | Limpa ambos: `app/chamados/services/disputa_emissao.py:1593`, `:1630-1632`. |
| `ressuscitar_perguntas` | Limpa `respondida_em` via lote: `app/chamados/models.py:1567`; update efetivo em `:3289-3291`. |
| `_desfazer_em_memoria` | Restauração em memória do carimbo anterior, sem save próprio: `app/chamados/models.py:3184-3191`. |
| `_universo_da_ressuscitacao` | Sondador em cópia, sem persistir: `app/chamados/models.py:1495-1509`. |

A porta compartilha o predicado: `app/chamados/services/fio.py:1578-1588` → `app/chamados/juizes.py:1515-1518`. O único consumidor produtivo encontrado do automático fornece texto constante não vazio: `app/api/foto_recebida.py:25`, `:48`.

**Escape estrutural:** `PerguntaDisputa.save` só deriva identidade e abre atomic; não valida HX-PAR-RESPOSTA (`app/chamados/models.py:3438-3448`). `descarimbar_em_lote(**outros)` também aceita campos adicionais sem validar esse par (`app/chamados/models.py:3277-3291`). São capacidades de contornar a guarda; escritor produtivo concreto que as use para gerar novo carimbo sem texto, além dos dois curados, **[nao verificado]**. A própria fixture cria estado ilegal por `objects.create`, explicitamente fora das portas (`app/chamados/tests/test_e3_10_cura_no_escritor.py:39-42`).

O bulk produtivo localizado altera apenas lavra: `app/chamados/veredito_lavrado.py:237`, `:250`. Os receivers da pergunta fecham beco ou emitem evento, sem atribuir o par nesses corpos (`app/chamados/signals.py:229-259`).

**Q5 — mutante plausível sem mordida no teste novo:** condicionar a guarda de `_nascer` a `if conversa:`. O caminho factual `abrir` ficaria sem recusa, embora ambos os caminhos hoje convirjam para `_nascer` (`app/chamados/models.py:2809`, `:2821`, `:2840`). Os testes novos do nascimento só exercitam `abrir_conversa` (`app/chamados/tests/test_e3_10_cura_no_escritor.py:85-110`). O teste expand extrai apenas os nós de save/registrar, sem executar a guarda (`app/chamados/tests/test_e4_expand.py:260-279`).

Deveria morder um RED chamando **`PerguntaDisputa.abrir` factual**, com carimbo e texto vazio, inclusive reuso de fato existente, afirmando recusa e nenhuma alteração. Sobrevivência executada contra todos os testes do diff **[nao verificado]**. [nao verificado]

## 12/11 15:58 · sessao 89593e35bd52 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

Cura entregue para certificação: **29 testes locais verdes, 14 mutantes mortos por asserção e Ruff limpo nos cinco Python tocados**. HEAD continua na base, sem commit. ([testes](docs/lastro_c19_execucao.txt:1040), [mutantes](docs/lastro_c19_execucao.txt:1086), [Ruff](docs/lastro_c19_execucao.txt:1045), [HEAD](docs/lastro_c19_execucao.txt:1053))

| Item | RED que mordeu antes da cura |
|---|---|
| 1 | `test_reconciliacao_mesmo_minuto_remove_so_instante_alvo`: célula preenchida e órfã remanescente conferidas. [RED](docs/lastro_c19_execucao.txt:96) |
| 2 | `test_dryrun_nao_orfa_no_mesmo_minuto_nao_vira_intervalo`. Também há cobertura com matcher real. [RED](docs/lastro_c19_execucao.txt:159), [matcher real](ponto/tests/test_lastro_c19_orfas_instantes.py:121) |
| 3 | `test_contador_sem_chamador_foi_apagado`: contador removido com lápide de uma linha. [RED](docs/lastro_c19_execucao.txt:193), [lápide](ponto/services/cartorio.py:721) |
| 4 | `test_leitor_filtra_par_hora_instante_junto`: filtra o par; ata antiga conserva instantes vazios. [RED](docs/lastro_c19_execucao.txt:213), [cura](escala/services/leitor_celula.py:187) |
| 5 | **Procede.** `test_sem_marcos_publica_orfas_da_ata_e_pdf_declara_ignorancia`: adaptador conserva horas sem marcos; PDF mantém “não sei” para ata antiga. [RED](docs/lastro_c19_execucao.txt:268), [cura](escala/utils.py:1105) |

Comandos para repetir os testes e os cinco mutantes C19: [nao verificado]

```bash
c19() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B \
    -m ponto.tests.test_lastro_c19_orfas_instantes "$@" -v
}

c19 ReconciliacaoC19Test
c19 DryRunC19Test
c19 ContadorC19Test
c19 LeitorC19Test.test_leitor_filtra_par_hora_instante_junto
c19 LeitorC19Test.test_sem_marcos_publica_orfas_da_ata_e_pdf_declara_ignorancia

c19 --mutante reconciliacao_minuto ReconciliacaoC19Test
c19 --mutante dryrun_minuto DryRunC19Test
c19 --mutante contador_volta ContadorC19Test
c19 --mutante leitor_desalinhado LeitorC19Test
c19 --mutante sem_marcos_descarta LeitorC19Test
```

Os comandos executados antes de cada cura, os nove mutantes C18 restantes e o comando exato do Ruff estão no [registro de entrega](docs/lastro_c19_execucao.txt:1065).

Arquivos::funções alterados:

- `escala/utils.py::montar_grade_prevista_periodo_por_turno` — comparação por instante no [dry-run](escala/utils.py:1699) e na [reconciliação](escala/utils.py:2067).
- `escala/utils.py::montar_realizado_grade` — [órfãs independem dos marcos](escala/utils.py:1097).
- `escala/services/leitor_celula.py::_orfas_da_ata` e `::grade_da_celula` — [filtragem pareada](escala/services/leitor_celula.py:180), [transporte](escala/services/leitor_celula.py:424).
- `ponto/services/cartorio.py::contar_celulas_orfas_sem_instantes` — [apagada](ponto/services/cartorio.py:721).
- Testes C18 ajustados e C19 acrescentados: [lista de funções e remoções](docs/lastro_c19_execucao.txt:1095).

| Campos | Quem escreve | Quem lê |
|---|---|---|
| Grade `orfas` / `orfas_instantes` | Matcher, builders e reconciliação: [mapa completo](docs/lastro_c19_execucao.txt:1107) | [Cartório](ponto/services/cartorio.py:737), [adaptador](escala/utils.py:1105), [espelho](ponto/services/espelho.py:860) |
| Ata `orfas` / `orfas_instantes` | [ata_do_dia](ponto/services/cartorio.py:739); porta persiste | [leitor](escala/services/leitor_celula.py:187) |
| Grade lida | [grade_da_celula](escala/services/leitor_celula.py:427) | [adaptador/espelho](ponto/services/espelho.py:704) |
| `orfas_grade` / `orfas_instantes_grade` | [montar_dias](ponto/services/espelho.py:859) | [PDF](relatorios/pdf_espelho.py:176), [tela](templates/ponto/espelho.html:240) |

Certificação Code/Opus, dados de produção, relavratura, DIFF monetário e bytes do PDF: **[nao verificado]**.

## 12/11 15:44 · sessao 8296f491b2c5 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVADO para fechar o aval (A): o cartório passou a lavrar os instantes, mas sobraram identidades de órfã por hora, há remoção semanticamente errada na reconciliação e o contador não tem chamador de produção.** (`app/ponto/services/cartorio.py:769`; `app/escala/utils.py:1721`; `app/escala/utils.py:2097`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:226`)

1. **O delta realiza (A) e só ele?**

Realiza a cadeia central: grade conserva ISO → cartório lavra → leitor transporta → espelho publica → PDF compara timestamps completos. Nesse caminho, o PDF exige offset e conserva segundos, sem atribuir à órfã a data da linha. (`app/escala/utils.py:628`; `app/ponto/services/cartorio.py:769`; `app/escala/services/leitor_celula.py:422`; `app/ponto/services/espelho.py:860`; `app/relatorios/pdf_espelho.py:182`; `app/relatorios/pdf_espelho.py:500`)

**Entretanto, sobraram estas comparações/chaves:**

| Sítio | Resíduo |
|---|---|
| PDF | Constrói `_espurias_datadas` por `DD/MM HH:MM` e elimina anomalias por substring dessa chave. É decisão, além de formatação. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`) |
| Dry-run de intervalo | Chama `_match_marcos` sem coletor, converte órfãs em conjunto de HH:MM e seleciona batidas pela pertença nesse conjunto. (`app/escala/utils.py:1696`; `app/escala/utils.py:1697`; `app/escala/utils.py:1721`) |
| Reconciliação | Reconhece a borda por HH:MM, relaciona órfãs ao turno por horas e remove a primeira ocorrência daquela hora. (`app/escala/utils.py:2062`; `app/escala/utils.py:2085`; `app/escala/utils.py:2089`; `app/escala/utils.py:2097`) |

Portanto, a retirada do casamento central de c17 aconteceu, mas **não houve eliminação de toda identidade por `(data, HH:MM)` ou HH:MM nos sítios perguntados**. Os resíduos acima continuam executáveis. (`app/relatorios/pdf_espelho.py:702`; `app/escala/utils.py:1721`; `app/escala/utils.py:2097`)

A relavratura da competência 11, reversão ANTES, prova de frota DEPOIS, DIFF monetário zero e contador real zero **[nao verificado]**: o dado fornecido não contém esses resultados.

2. **ESMERIL-SEMÂNTICA: escritores, leitores e acompanhamento**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `dia['orfas_instantes']` | `_match_marcos` preenche coletores nas duas saídas; builders publicam e acrescentam sobras. (`app/escala/utils.py:628`; `app/escala/utils.py:671`; `app/escala/utils.py:799`; `app/escala/utils.py:811`; `app/escala/utils.py:1863`; `app/escala/utils.py:1888`) | Ordenação, reconciliação e `ata_do_dia`. (`app/escala/utils.py:815`; `app/escala/utils.py:1892`; `app/escala/utils.py:2099`; `app/ponto/services/cartorio.py:769`) |
| `ata['orfas_instantes']` | Produtor de conteúdo: `ata_do_dia`. A porta copia/persiste a ata recebida. (`app/ponto/services/cartorio.py:769`; `app/ponto/portas/celula.py:995`; `app/ponto/portas/celula.py:1033`) | Contador e `grade_da_celula`. (`app/ponto/services/cartorio.py:745`; `app/escala/services/leitor_celula.py:422`) |
| `dia['orfas_instantes']` transportado | Leitor copia da ata; degradação pelo builder esvazia deliberadamente. (`app/escala/services/leitor_celula.py:422`; `app/ponto/services/espelho.py:503`) | Espelho publica como `orfas_instantes_grade`. (`app/ponto/services/espelho.py:860`) |
| `orfas_instantes_grade` | `montar_dias`. (`app/ponto/services/espelho.py:860`) | `_orfas_publicadas`, que produz instantes para classificação das batidas e entradas espúrias. (`app/relatorios/pdf_espelho.py:177`; `app/relatorios/pdf_espelho.py:195`; `app/relatorios/pdf_espelho.py:500`; `app/relatorios/pdf_espelho.py:590`) |
| `orfas_sem_instantes` | Coletor do PDF, quando o helper devolve `None`. (`app/relatorios/pdf_espelho.py:587`) | Guarda da marca `b.orfa` e observação textual. (`app/relatorios/pdf_espelho.py:590`; `app/relatorios/pdf_espelho.py:899`) |

O contrato preserva **instante absoluto com offset e segundos**: os timestamps entram por `timezone.localtime`, são serializados por `isoformat` e o PDF rejeita valores sem offset. A linha não determina a data do instante: há órfãs atribuídas ao turno e sobras destinadas pelo dia civil. (`app/escala/utils.py:692`; `app/escala/utils.py:1367`; `app/escala/utils.py:628`; `app/relatorios/pdf_espelho.py:186`; `app/escala/utils.py:807`; `app/escala/utils.py:1872`)

A ordenação é **por HH:MM, com pares preservados**, não por cronologia absoluta. Append, promoção de célula para órfã, clip, folga, ausência/justificativa e legacy acompanham o campo nos caminhos examinados. (`app/escala/utils.py:815`; `app/escala/utils.py:1770`; `app/escala/utils.py:1771`; `app/escala/utils.py:978`; `app/escala/utils.py:983`; `app/escala/utils.py:1173`; `app/escala/utils.py:1252`; `app/escala/utils.py:1296`; `app/escala/utils.py:1302`; `app/escala/utils.py:1062`)

**A reconciliação acompanha a posição, mas pode acompanhar a batida errada.** Contraexemplo executado em memória no laço real: órfãs `03:00:10` e `03:00:20`, borda alvo `03:00:20`. O `index('03:00')` remove `:10`; a célula recebe `:20`, que também permanece órfã. Comprimentos continuam iguais, portanto o contador não acusa. Batidas de tipos diferentes podem sobreviver ao dedupe nesse mesmo minuto. (`app/escala/utils.py:2097`; `app/escala/utils.py:2099`; `app/escala/utils.py:2101`; `app/ponto/services/cartorio.py:746`; `app/escala/utils.py:1378`)

Há ainda dois desacoplamentos:

- O leitor filtra horas falsas em `_orfas_da_ata`, mas copia instantes sem aplicar o mesmo filtro; atas malformadas podem perder o pareamento. Ocorrência em produção **[nao verificado]**. (`app/escala/services/leitor_celula.py:187`; `app/escala/services/leitor_celula.py:422`)
- A degradação mantém horas e apaga instantes de propósito: nesse campo, `[]` também representa ausência de lastro lavrado. (`app/ponto/services/espelho.py:502`; `app/ponto/services/espelho.py:503`)

3. **Segundo escritor ou lavra sem `ata_do_dia`**

Não encontrei segundo **produtor do conteúdo** `ata['orfas_instantes']`: ele nasce em `ata_do_dia`; os dois caminhos de lavra com ata chegam desse produtor à porta. A porta copia o conteúdo, inclusive quando prevalece o veredito administrativo. (`app/ponto/services/cartorio.py:769`; `app/ponto/services/cartorio.py:1280`; `app/ponto/services/cartorio.py:1321`; `app/ponto/services/cartorio.py:1880`; `app/ponto/services/cartorio.py:1882`; `app/ponto/portas/celula.py:1006`)

**Existe gravação de ata sem passar por ele:** `carimbar_precedencia` pode partir de ata vazia, adicionar `precedencia` e salvar. Isso é outro dono por chave, não outro produtor de instantes. Também existe ata parcial construída em cópia de memória pelo builder, sem instantes; aquele trecho não persiste a cópia. (`app/ponto/portas/celula.py:1083`; `app/ponto/portas/celula.py:1087`; `app/ponto/portas/celula.py:104`; `app/escala/utils.py:1943`; `app/escala/utils.py:1952`)

4. **Contador: chamadores e autoridades**

**L-111: sítio com zero chamador de produção.** No censo, os chamadores encontrados são testes; o comando de produção chama `julgar_colab`, sem incorporar esse contador. Assim, sua existência não constitui vigia operacional. (`app/ponto/services/cartorio.py:721`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:226`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:323`; `app/ponto/management/commands/processar_cartorio.py:71`; `app/docs/LEIS.md:137`)

As autoridades usadas são as corretas: empresa/janela delimitam as células; `janela_competencia` delega a `janela_fechamento`; K9 decide por colaborador, compondo exportação vigente e holerite de lote publicado. K9 está registrado. (`app/ponto/services/cartorio.py:728`; `app/ponto/services/cartorio.py:731`; `app/ponto/services/cartorio.py:736`; `app/ponto/services/cartorio.py:739`; `app/folha/export.py:51`; `app/folha/export.py:618`; `app/folha/export.py:624`; `app/core/juizes.py:337`)

Mas ele mede somente **células com `n_orfas>0` e lista ausente/de tamanho diferente**. `['invalido']` com `n_orfas=1` passa, embora o PDF rejeite; ordem, identidade e offset não são certificados pelo zero desse contador. (`app/ponto/services/cartorio.py:744`; `app/ponto/services/cartorio.py:746`; `app/relatorios/pdf_espelho.py:182`)

5. **Classes que já reprovaram**

- **Juiz/derivação paralela:** o PDF ainda decide supressão de anomalias pela chave textual reduzida; a reconciliação identifica órfã por minuto. São os resíduos concretos encontrados. (`app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`; `app/escala/utils.py:2063`)
- **Segundo escritor:** não demonstrado para o conteúdo novo; há persistência de outra chave da ata pela porta, conforme item 3. (`app/ponto/services/cartorio.py:769`; `app/ponto/portas/celula.py:1084`)
- **Relógio:** os helpers novos usam dados recebidos; o PDF captura `agora` nas entradas públicas. Não identifiquei nova leitura fora da borda nesses trechos. (`app/relatorios/pdf_espelho.py:174`; `app/relatorios/pdf_espelho.py:1028`; `app/relatorios/pdf_espelho.py:1075`)
- **Travas:** não há aquisição nova de trava nos trechos acrescentados; não encontrei inversão atribuível a c18. Concorrência em execução **[nao verificado]**. (`app/ponto/services/cartorio.py:721`; `app/ponto/services/cartorio.py:769`; `app/relatorios/pdf_espelho.py:174`)
- **Campo com significados incompatíveis:** preservação de segundos no transporte não combina com a identificação por minuto usada para remover a órfã. (`app/escala/utils.py:628`; `app/escala/utils.py:2097`)

**Os REDs comportamentais mordem o retorno do B central.** Repondo em memória os dois assignments c17 do anexo, os cinco testes de PDF produziram oito falhas, contando subtests: noturno, ata antiga, segundos/offset e valores inválidos. Porém, o mutante B não está incorporado na lista, e o selo estrutural aceita explicitamente `(data, HH:MM)` e `DD/MM HH:MM`. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:124`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:138`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:145`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:153`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:373`; `app/relatorios/tests/test_pdf_identidade_datada.py:301`; `app/relatorios/tests/test_pdf_identidade_datada.py:302`)

O teste de reconciliação verifica só a remoção posicional: exclui o preenchimento da célula e não fornece o instante alvo. Não morde o contraexemplo do item 2. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:344`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:345`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:349`)

Execução nesta leitura, sem log criado: **15 testes C18 verdes; os dez mutantes incorporados falharam; sete testes de identidade datada verdes**. Esses ensaios não certificam banco nem bytes do PDF. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:1`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:373`; `app/relatorios/tests/test_pdf_identidade_datada.py:229`)

6. **Competência exportada**

**Existe caminho para mudar o gravado de exportada:** `processar_cartorio --apply --forcar` seleciona células passadas sem recorte por competência fechada; a lavra acrescenta a nova chave e uma ata diferente é persistida. O alcance do comando já existia, mas o conteúdo acrescentado por c18 passa por ele. (`app/ponto/management/commands/processar_cartorio.py:32`; `app/ponto/management/commands/processar_cartorio.py:34`; `app/ponto/management/commands/processar_cartorio.py:54`; `app/ponto/management/commands/processar_cartorio.py:71`; `app/ponto/services/cartorio.py:769`; `app/ponto/services/cartorio.py:1882`; `app/ponto/portas/celula.py:1033`)

Sem `forcar`, impressão igual continua pulando a célula; adicionar o campo não prova relavratura automática das atas antigas. Execução sobre exportadas, mudança efetiva de TXT/dinheiro ou DIFF zero **[nao verificado]**. (`app/ponto/services/cartorio.py:1438`; `app/ponto/services/cartorio.py:1442`)

O PDF diz “não sei”, sem casar por hora **na classificação central**, quando recebe horas órfãs e faltam instantes válidos. Mas isso não é universal: com marcos vazios, o adaptador descarta também as horas; o helper interpreta horas vazias como conjunto conhecido vazio e não acende o aviso. Ocorrência em produção **[nao verificado]**. (`app/escala/utils.py:1103`; `app/ponto/services/espelho.py:704`; `app/ponto/services/espelho.py:859`; `app/relatorios/pdf_espelho.py:178`; `app/relatorios/pdf_espelho.py:587`; `app/relatorios/pdf_espelho.py:899`)

7. **Leis contrariadas**

**L-111 é contrariada pelo contador sem chamador de produção.** A regra exige retirar o sítio sem chamador; a função permanece sustentada pelos testes. (`app/docs/LEIS.md:137`; `app/ponto/services/cartorio.py:721`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:226`)

**LEI-AKITA 2/L-038 seguem contrariadas no filtro próprio de anomalias do PDF:** ele deriva uma identidade reduzida para decidir o que suprimir. A exigência de RED também fica incompleta para a identidade da reconciliação, embora os REDs centrais tenham mordida. (`CLAUDE.md:22`; `app/docs/LEIS.md:86`; `app/relatorios/pdf_espelho.py:698`; `app/relatorios/pdf_espelho.py:702`; `CLAUDE.md:30`; `app/ponto/tests/test_lastro_c18_orfas_instantes.py:345`)

O cumprimento integral do aval — sobretudo preservar exportadas e provar o zero monetário após relavratura da 11 — permanece **[nao verificado]**.

## 12/11 15:42 · sessao 0732f0ee3ea7 · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **L-111: sítio com zero chamador de produção.** A função só é definida em `app/ponto/services/cartorio.py:721`; os chamadores encontrados estão nos testes (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:226`, `:323`). O contador não é incluído nos contadores do cartório (`app/ponto/services/cartorio.py:1983`) nem chamado pelo command (`app/ponto/management/commands/processar_cartorio.py:71`). A contagem real na competência aberta permanece **[nao verificado]**.
- As autoridades do contador são corretas: janela via `janela_competencia` (`app/ponto/services/cartorio.py:727`), que delega para `janela_fechamento` (`app/folha/export.py:47`); fechamento por pessoa via K9 (`app/ponto/services/cartorio.py:739`), que compõe `competencia_entregue` e holerite de lote publicado (`app/folha/export.py:617`). K6/K9 estão registrados em `app/core/juizes.py:327` e `:337`.
- **O contador só certifica formato de lista/tamanho contra `n_orfas`.** Não valida ISO, offset, segundos ou correspondência posicional (`app/ponto/services/cartorio.py:743`). Uma lista `['invalido']` com `n_orfas=1` conta zero, embora o PDF a rejeite (`app/relatorios/pdf_espelho.py:182`).

Escritores e caminhos da ata:

- Único produtor explícito do conteúdo `ata['orfas_instantes']`: `ata_do_dia`, `app/ponto/services/cartorio.py:769`.
- Lavra normal: `ata_do_dia` → `_ata` → `lavrar_veredito`, `app/ponto/services/cartorio.py:1880`, `:1882`. Ramo sem adesão: `:1280`, `:1321`.
- A porta copia e compara a ata (`app/ponto/portas/celula.py:995`), escreve mesmo quando o veredito humano prevalece (`:1006`) ou no ramo comum (`:1033`). Não deriva instantes.
- `celula_como_o_julgamento_deixa` só copia em memória (`app/ponto/portas/celula.py:942`, `:945`). O builder também fabrica uma ata parcial em cópia de memória, só com lâmpadas e horas, sem instantes (`app/escala/utils.py:1943`, `:1944`, `:1952`).
- **Existe gravação de ata sem passar por `ata_do_dia`:** `carimbar_precedencia` parte de `dict(cel.ata or {})`, adiciona a chave e salva (`app/ponto/portas/celula.py:1083`, `:1087`). É outro dono declarado por chave (`:104`), não segundo produtor de instantes.
- Admin (`app/ponto/services/veredito_celula.py:351`) e complemento de status (`app/ponto/services/cartorio.py:964`) chamam a lavra sem ata; nesse caso a porta não substitui a ata (`app/ponto/portas/celula.py:991`). Quando há ata, só `precedencia` é preservada da anterior (`:997`, `:104`).
- Não encontrei `_gravar_sem_fato` na cópia **[nao verificado quanto a outras versões]**.

Competência exportada:

- **Não afirmar impossibilidade de mudar o gravado.** O command aceita `--apply --forcar` (`app/ponto/management/commands/processar_cartorio.py:32`, `:34`), seleciona todas as células passadas (`:54`, `:66`) e encaminha `forcar` (`:71`). A lavra resultante acrescenta o campo novo (`app/ponto/services/cartorio.py:1880`) e a porta persiste uma ata diferente (`app/ponto/portas/celula.py:1000`, `:1033`). Não há guarda K9 nesse caminho; o uso K9 no cartório está no contador (`app/ponto/services/cartorio.py:727`).
- Esse alcance do command já existe; o efeito acrescentado por C18 é a nova chave na ata lavrada (`app/ponto/services/cartorio.py:769`). Execução sobre exportadas, TXT ou dinheiro em produção: **[nao verificado]**.
- O patch não altera a versão declarada (`app/ponto/services/cartorio.py:132`). Células com impressão igual continuam puladas sem `forcar` (`:1438`, `:1442`; ramo sem adesão `:1289`, `:1293`), portanto o campo novo não prova relavratura automática de atas antigas.
- Para o PDF, o fallback descarta deliberadamente os instantes reconstruídos pelo builder (`app/ponto/services/espelho.py:502`), inclusive o caminho de ata agregada (`:547`); a publicação da tela apenas copia o campo (`:860`).

## 12/11 15:42 · sessao 35878f3eb069 · - · - · origem=Ronald (tmux)

Achados prioritários:

- **Sobrou identidade de órfã por HH:MM no dry-run de intervalo:** `_match_marcos` é chamado sem coletor de instantes; `_orf2` vira `set` de horas e seleciona batidas com `strftime('%H:%M') in _orfset`. Portanto, o c18 não eliminou todas essas comparações de `escala/utils.py`. (`app/escala/utils.py:1696`, `app/escala/utils.py:1697`, `app/escala/utils.py:1721`)
- **Sobrou identidade por HH:MM na reconciliação:** testa presença da borda pela hora; relaciona órfãs e células ao turno por conjuntos/comparações de horas; remove a primeira ocorrência dessa hora. (`app/escala/utils.py:2062`, `app/escala/utils.py:2063`, `app/escala/utils.py:2085`, `app/escala/utils.py:2089`, `app/escala/utils.py:2091`, `app/escala/utils.py:2097`)
- **Bug semântico reproduzido em memória executando o laço AST real:** duas órfãs `03:00:10` e `03:00:20`, alvo da reconciliação `03:00:20`: `index('03:00')` remove a órfã de `:10`; a célula passa a conter `:20`, que também permanece em `orfas_instantes`. As listas continuam com tamanhos e posições pareados, mas apontam para a batida errada. O mecanismo é `index` por HH:MM seguido de `pop` posicional e preenchimento pelo instante alvo. (`app/escala/utils.py:2097`, `app/escala/utils.py:2099`, `app/escala/utils.py:2100`, `app/escala/utils.py:2101`)
- O dedupe não impede necessariamente esse caso: só elimina proximidade quando o tipo é igual; batidas E/S distintas podem sobreviver no mesmo minuto. (`app/escala/utils.py:1376`, `app/escala/utils.py:1378`)
- **O RED da reconciliação não testa essa identidade:** recorta somente desde `_ix_orfa`, exclui o preenchimento da célula e não fornece o instante alvo; valida a remoção da primeira ocorrência. (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:344`, `app/ponto/tests/test_lastro_c18_orfas_instantes.py:345`, `app/ponto/tests/test_lastro_c18_orfas_instantes.py:349`, `app/ponto/tests/test_lastro_c18_orfas_instantes.py:351`)

Censo e significado:

| Operação | Arquivo:linha |
|---|---|
| Origem dos timestamps em fuso local | `app/escala/utils.py:692`, `app/escala/utils.py:1367` |
| Célula conserva ISO completo | `app/escala/utils.py:328` |
| `_match_marcos` escreve coletor de instantes, nas duas saídas | `app/escala/utils.py:628`, `app/escala/utils.py:671` |
| Builders inicializam coletores | `app/escala/utils.py:785`, `app/escala/utils.py:1747` |
| Builders publicam horas e instantes | `app/escala/utils.py:799`, `app/escala/utils.py:1862`, `app/escala/utils.py:1863` |
| Célula promovida a órfã leva instante | `app/escala/utils.py:1770`, `app/escala/utils.py:1771`, `app/escala/utils.py:1782`, `app/escala/utils.py:1783` |
| Sobras são destinadas pelo dia civil | `app/escala/utils.py:807`, `app/escala/utils.py:1872` |
| Sobras acrescentam as duas listas | `app/escala/utils.py:810`, `app/escala/utils.py:811`, `app/escala/utils.py:1884`, `app/escala/utils.py:1885`, `app/escala/utils.py:1887`, `app/escala/utils.py:1888` |
| Sort conserva pares, ordenando por HH:MM | `app/escala/utils.py:815`, `app/escala/utils.py:816`, `app/escala/utils.py:817`, `app/escala/utils.py:1892`, `app/escala/utils.py:1893`, `app/escala/utils.py:1894` |
| Clip limpa ambas | `app/escala/utils.py:977`, `app/escala/utils.py:978`, `app/escala/utils.py:982`, `app/escala/utils.py:983` |
| Folga limpa ambas | `app/escala/utils.py:1172`, `app/escala/utils.py:1173` |
| Ausência/justificativa limpam ambas | `app/escala/utils.py:1251`, `app/escala/utils.py:1252`, `app/escala/utils.py:1295`, `app/escala/utils.py:1296`, `app/escala/utils.py:1301`, `app/escala/utils.py:1302` |
| Legacy conserva o campo pelo espalhamento de `sd` | `app/escala/utils.py:1062` |
| Leitor inicializa lista vazia | `app/escala/services/leitor_celula.py:390`, `app/escala/services/leitor_celula.py:391` |
| Leitor lê ata e escreve dia | `app/escala/services/leitor_celula.py:421`, `app/escala/services/leitor_celula.py:422` |

Os instantes preservam segundos/offset pelo `isoformat`; a ordem das listas é pareada, mas não necessariamente cronológica absoluta: o sort usa a hora exibida. A linha pode conter órfã do turno e sobra destinada pelo dia civil, portanto sua data não substitui a data do instante. (`app/escala/utils.py:628`, `app/escala/utils.py:671`, `app/escala/utils.py:807`, `app/escala/utils.py:815`, `app/escala/utils.py:1872`)

Duas ressalvas adicionais:

- `_orfas_da_ata` filtra valores falsos das horas, enquanto `orfas_instantes` é copiado independentemente. Uma ata malformada pode perder o pareamento; existência desse dado em produção: [nao verificado]. (`app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:422`)
- Há reconstrução de **ata temporária em memória**, contendo `lampadas` e `orfas`, sem instantes, para alimentar o juiz de turnos; não é gravação persistente nesse trecho. (`app/escala/utils.py:1943`, `app/escala/utils.py:1944`, `app/escala/utils.py:1949`, `app/escala/utils.py:1952`, `app/escala/utils.py:1955`)

## 12/11 15:42 · sessao b2616b5b9103 · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **Sobrou casamento por data + HH:MM no PDF.** `_espurias_datadas` é um conjunto `strftime('%d/%m %H:%M')` das batidas marcadas órfãs, e `_anom` elimina anomalias por substring dessa chave. Perde ano e segundos; não é apenas apresentação. `app/relatorios/pdf_espelho.py:698-702`. O selo aceita explicitamente essa classe de chave: `app/relatorios/tests/test_pdf_identidade_datada.py:301-302`.

2. **O caminho central C18 deixou de derivar a data da linha.** `_orfas_publicadas` interpreta os ISO, exige offset e comprimento igual às horas; o coletor casa `b.timestamp` com esses instantes e a marca viaja para o desenho. `app/relatorios/pdf_espelho.py:174-188`, `:494-500`, `:587-590`, `:944-947`. O espelho transporta o campo em `app/ponto/services/espelho.py:860` e apaga instantes do builder sem lavra em `:502-503`.

3. **“Ata antiga diz não sei” é condicional às horas publicadas.** O helper retorna conjunto vazio imediatamente quando `orfas_grade` está vazia, sem conferir ausência dos instantes. `app/relatorios/pdf_espelho.py:178-180`. Há caminho de código que esvazia horas: adaptador com `marcos=[]` devolve `[], []`, `app/escala/utils.py:1103-1104`; espelho publica esse resultado em `app/ponto/services/espelho.py:704-707`, `:859`. Sem escala elegível nem instala o adaptador, `:487-491`. Nesse caminho o flag fica falso, `app/relatorios/pdf_espelho.py:587`, e não entra o aviso, `:899-900`. Ocorrência em produção: **[nao verificado]**. Os testes da antiga/degradação fornecem horas explicitamente: `app/ponto/tests/test_lastro_c18_orfas_instantes.py:118-143`, `:353-368`.

4. **Os REDs comportamentais mordem o retorno do B central.** Executei os cinco `PdfC18Test` com substituição AST em memória dos dois assignments pelos trechos c17 do anexo: oito falhas contando subtests, zero erros. Mordem o noturno (`app/ponto/tests/test_lastro_c18_orfas_instantes.py:129`), ata antiga (`:142`), segundos/offset (`:151`) e inválidos (`:161`). O caso D/D+1 (`:131-136`) passa B porque B já diferencia a data. O mutante B não está na lista incorporada, `:373`, e o selo estrutural permite data + HH:MM, `app/relatorios/tests/test_pdf_identidade_datada.py:301`.

5. **Ensaios atuais executados sem escrita:** C18: 15 testes OK; identidade datada: sete testes OK. Escopo declarado sem banco/rede/Django/PDF bytes: `app/ponto/tests/test_lastro_c18_orfas_instantes.py:1-4`; limitações do censo AST: `app/relatorios/tests/test_pdf_identidade_datada.py:78-84`. A suite verde não elimina o resíduo de `pdf_espelho.py:698-702`.

6. **Relógio do PDF permanece na borda pública:** captura e repassa `agora` em `app/relatorios/pdf_espelho.py:1028-1030` e `:1075`; helper novo usa apenas dados recebidos, `:174-188`. Não identifiquei nova leitura de relógio nos trechos C18 desses dois arquivos.

## 12/11 15:38 · sessao 876dbbcd439a · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

C18 implementada; censo, REDs, comandos e limites estão no [relatório](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c18_relatorio.txt:1).

- **86 testes sem banco passaram; 14 mutantes morderam.** Contador: `0`; retirando o instante do escritor: `1`. [Evidências](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c18_relatorio.txt:5) [nao verificado]
- Suite Django e Ruff ficaram bloqueados por dependências ausentes; entrega sem certificação. [Limites](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c18_relatorio.txt:8) [nao verificado]
- Sem commit ou relavra, conforme restrições; frota e DIFF financeiro **[nao verificado]**. [Registro](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_c18_relatorio.txt) [nao verificado]

## 12/11 15:37 · sessao 83ad142c7d01 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · sonda · origem=Code

Reproduzo a saída raspada fornecida. Nova execução, código e predicados exatos de `sonda_numeros.py`: **[nao verificado]**.

```text
sonda_leitura: sonda_numeros.py [919e00d77781d52f] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

hoje 2026-11-12
futuras_total 8721
futuras_por_origem [('gerada',)] {'gerada': 8721, 'editada': 0}
D1_futura_com_julgamento 0 colabs 0
D1_regenerada_apos_julgada 0
D3_futura_gerada_sem_geradora 0 colabs 0 trabalha_true 0
D7_vinculo_inativo_com_fim_futuro 2 colabs 2
  D7 ec#489 col574 ini=2024-01-01 fim=2027-07-06 celulas_futuras_dela=236
  D7 ec#955 col203 ini=2026-07-21 fim=2026-12-31 celulas_futuras_dela=0
total_erros 0
```

**`total_erros = 0`**, conforme o dado fornecido; conferência independente: **[nao verificado]**. Os zeros reportados para D1/D3 não demonstram impossibilidade desses estados nem medem lacunas por pessoa/data em D7. **[nao verificado quanto à abrangência da sonda]**

**D1 — caminho possível no código**

- A entrada unitária `julgar_celula` recusa `data > hoje`, retornando `None`. (`app/ponto/services/cartorio.py:2016`; `app/ponto/services/cartorio.py:2025`)
- A entrada de lote `julgar_colab` recebe `cels` do chamador e repassa a lista ao corpo sem filtrar futuras. (`app/ponto/services/cartorio.py:940`; `app/ponto/services/cartorio.py:996`)
- Um caminho concreto é o ramo `adesao is None`: percorre as células recebidas; com `apply_=True`, `completar_status=False` e vencido o pulo de idempotência — por exemplo, `forcar=True` — chama `lavrar_veredito(..., save=False, ata=_ata_nb)` e depois `carimbar(..., lavrados=_cv)`. (`app/ponto/services/cartorio.py:1211`; `app/ponto/services/cartorio.py:1261`; `app/ponto/services/cartorio.py:1264`; `app/ponto/services/cartorio.py:1268`; `app/ponto/services/cartorio.py:1292`; `app/ponto/services/cartorio.py:1314`)
- As duas operações da porta não validam `cel.data`: a lavra atribui ata/veredito quando aplicável; o carimbo atribui `julgada_em=agora` e persiste também os campos lavrados. Ata preenchida nesse ramo depende de `_ata_nb` não nula e diferente da atual. A chamada direta dessas operações também permite essa sequência. (`app/ponto/portas/celula.py:991`; `app/ponto/portas/celula.py:1032`; `app/ponto/portas/celula.py:1039`; `app/ponto/portas/celula.py:1065`; `app/ponto/portas/celula.py:1070`)

Isso estabelece possibilidade estática. Um chamador de produção ter entregue uma futura ao lote, ou executado diretamente essa sequência: **[nao verificado]**.

A regeneração pode conservar julgamento previamente preenchido: seu save altera desenho e marcas de regeneração, sem ata/veredito/julgada_em; o rejulgamento pula futuras. (`app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:276`; `app/ponto/portas/celula.py:431`)

**D7 — ausência em dia coberto**

O dado informa 236 futuras “dela” para `ec#489/col574` e zero para `ec#955/col203`. Isso **não comprova célula futura AUSENTE para nenhum dos dois**: não foram fornecidas datas nem o predicado desse contador. **[nao verificado]**

A identidade da célula é **colaborador/data**, não vínculo/data. Portanto, zero atribuídas à EC pode coexistir com células da pessoa cuja geradora seja outra EC ou nula; 236 também não certifica cobertura completa das datas. (`app/escala/models.py:1692`; `app/escala/models.py:1833`) Quais dessas situações existem para os IDs listados: **[nao verificado]**.

A possibilidade estrutural de D7 existe: o gerador seleciona apenas `ativa=True`, embora a autoridade admita vínculo inativo com fim declarado e cobertura inclusiva entre início e fim. (`app/ponto/portas/celula.py:829`; `app/ponto/portas/celula.py:849`; `app/escala/alimentacao.py:196`; `app/escala/alimentacao.py:214`) Para afirmar ausência real, falta verificar existência de célula por pessoa/data em cada dia futuro coberto; essa medição não aparece na saída. **[nao verificado]**

Há divergências materiais entre matriz e cópia: nesta árvore, o gerador reescreve futuras existentes quando necessário, e o ramo fora de vigência salva somente `trabalha/dna`, conservando a FK. Assim, as descrições de conservação pelo cron e de FK anulada em D3 não correspondem a esses trechos. (`app/ponto/portas/celula.py:870`; `app/ponto/portas/celula.py:876`; `app/ponto/portas/celula.py:238`; `app/ponto/portas/celula.py:245`)

## 12/11 15:36 · sessao af02dd476f08 · - · - · origem=Ronald (tmux)

D1 — caminho estático mínimo:

- A entrada unitária `julgar_celula` obtém `hoje = timezone.localdate()` e retorna `None` para `data > hoje`; portanto essa entrada não julga uma célula futura. (`app/ponto/services/cartorio.py:2016`; `app/ponto/services/cartorio.py:2025`)
- A entrada de lote `julgar_colab(c, cels, hoje=..., agora=..., apply_=...)` recebe a lista do chamador e a repassa ao corpo sem filtrar futuras. (`app/ponto/services/cartorio.py:940`; `app/ponto/services/cartorio.py:983`; `app/ponto/services/cartorio.py:996`)
- No corpo, havendo **`adesao is None`**, percorre todas as células recebidas. Com **`apply_=True`**, **`completar_status=False`** e sem o pulo por impressão idêntica (ou com `forcar=True`), chama `cel.lavrar_veredito(..., save=False, ata=_ata_nb)` e depois `carimbar(..., lavrados=_cv)`. Esse é um caminho conjunto que pode alcançar uma futura indevidamente incluída pelo chamador. (`app/ponto/services/cartorio.py:1211`; `app/ponto/services/cartorio.py:1261`; `app/ponto/services/cartorio.py:1264`; `app/ponto/services/cartorio.py:1268`; `app/ponto/services/cartorio.py:1292`; `app/ponto/services/cartorio.py:1314`)
- `lavrar_veredito` não verifica a data: quando há alteração, atribui `ata`, `veredito`, `veredito_via`, `veredito_em`; `save=False` deixa os campos para a operação seguinte salvar. Ata preenchida depende de `_ata_nb` não nula e diferente da atual. (`app/ponto/portas/celula.py:974`; `app/ponto/portas/celula.py:991`; `app/ponto/portas/celula.py:1032`; `app/ponto/portas/celula.py:1039`)
- `carimbar` também não verifica a data: atribui `julgada_em=agora` e salva junto os campos lavrados; o save é efetivado por `salvar_e_emitir`. Assim, chamada direta dessas operações também permite preencher uma futura, respeitadas suas outras guardas e condições de alteração. (`app/ponto/portas/celula.py:1052`; `app/ponto/portas/celula.py:1061`; `app/ponto/portas/celula.py:1065`; `app/ponto/portas/celula.py:1070`; `app/ponto/portas/celula.py:905`)

Isso demonstra possibilidade estática; chamador de produção fornecendo uma futura, execução desse caminho e existência de futura carimbada em produção: **[nao verificado]**.

A regeneração reescreve trabalho/FK/DNA e marcas de regeneração, sem incluir ata/veredito/julgada_em no save; o rejulgamento pula futuras. Portanto pode **conservar** um julgamento previamente preenchido sem o produzir por esse ato. (`app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:276`; `app/ponto/portas/celula.py:431`)

A matriz anexa diverge da cópia no ramo fora de vigência: aqui ele só grava `trabalha` e `dna`, conservando a geradora; não grava FK nula. (`app/ponto/portas/celula.py:238`; `app/ponto/portas/celula.py:243`; `app/ponto/portas/celula.py:245`)

## 12/11 15:36 · sessao a5e0c3951f1e · - · - · origem=Ronald (tmux)

D7 não comprova célula ausente para nenhum dos dois vínculos. O dado informa apenas `ec#489 col574 celulas_futuras_dela=236` e `ec#955 col203 celulas_futuras_dela=0`; o predicado exato desse contador não foi verificado, pois o código de `sonda_numeros.py` não está disponível. [nao verificado]

A célula pertence à identidade **colaborador/data**, com unicidade definida em `app/escala/models.py:1833`. Portanto, zero células atribuídas à EC não demonstra zero células da pessoa nas mesmas datas: elas podem existir com outra geradora ou geradora nula; quais existem para esses IDs permanece [nao verificado].

Na cópia, o gerador seleciona somente ECs `ativa=True` (`app/ponto/portas/celula.py:829`), depois exige cobertura (`app/ponto/portas/celula.py:849`), consulta células por pessoa/data e cria ausentes (`app/ponto/portas/celula.py:840`, `app/ponto/portas/celula.py:852`, `app/ponto/portas/celula.py:853`). A autoridade admite EC inativa com fim declarado e considera início/fim inclusivos (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`). Assim, a exclusão dessas ECs pelo gerador é demonstrável; ocorrência concreta de ausência por dia não foi medida no stdout fornecido. [nao verificado quanto à ocorrência]

O comando usa por padrão a união das competências correntes das empresas ativas (`app/ponto/management/commands/gerar_celulas.py:290`, `app/ponto/management/commands/gerar_celulas.py:291`) e chama essa porta (`app/ponto/management/commands/gerar_celulas.py:310`). O stdout não fornece datas ausentes, células de outras geradoras, janela efetivamente examinada para D7 ou escolha de autoridade por dia. [nao verificado]

**Divergência material do anexo:** `cadastro_futuro.py` e `vigia_cadastro_futuro.py` não apareceram na cópia pesquisada; atribuir-lhes comportamento nesta árvore fica [nao verificado]. Além disso, o gerador desta cópia reescreve células existentes de hoje/futuro quando desenho/geradora mudam (`app/ponto/portas/celula.py:870`, `app/ponto/portas/celula.py:872`, `app/ponto/portas/celula.py:876`), diferentemente da conservação descrita na matriz.

## 12/11 15:32 · sessao d123cf6bebfd · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · leitura · origem=Code

A cópia contém curas na origem, adaptações locais e transições ainda abertas. A candidata **(b)** está implementada; isso não basta para fechar os estados “futura carimbada”, “reativada por `update`” e “passada atingida por exclusão da geradora”. (`app/ponto/services/vigia_cadastro_futuro.py:31`; `app/colaboradores/services/vinculo.py:716`; `app/ponto/portas/celula.py:453`; `app/colaboradores/services/vinculo.py:1007`)

A comparação histórica exata `0a747193e..a5a92544a` e o resultado da suíte em c4b ficam **[nao verificado]**: a cópia disponível não trouxe histórico Git nem esse diff. Abaixo separo comportamento verificável nesta árvore de atribuição histórica e dado de produção. **[nao verificado]**

**1. Matriz de transições**

“Gerada/editada” é `origem`; “regenerada” acrescenta marcas de reescrita; “carimbada” acrescenta julgamento. Portanto, as cinco colunas pedidas não são estados exclusivos: uma célula regenerada pode continuar gerada e carimbada. A proteção de `origem='editada'` prevalece sobre as demais características. (`app/escala/models.py:1690`; `app/escala/models.py:1718`; `app/escala/models.py:1745`; `app/ponto/portas/celula.py:285`)

Nas matrizes, cada casa tem **ação atual / exigência / conclusão**. Os símbolos abaixo incorporam o comportamento e as referências indicadas; `=` significa concordância naquele recorte, `D` aponta divergência ou transição incompleta, e `?` significa que falta estabelecer a aplicação da lei. As exigências específicas dos avais transcritos são tomadas do dado fornecido, sem confirmação dos respectivos arquivos locais. **[nao verificado]**

| Símbolo | Expansão verificável usada nas casas |
|---|---|
| **I** | Conserva a célula: suplemento seleciona somente futuras existentes `origem='gerada'`; não cria ausente nem seleciona editada. (`app/ponto/services/cadastro_futuro.py:18`; `app/ponto/services/cadastro_futuro.py:22`) |
| **R** | Em dia coberto, compara `trabalha`, geradora e DNA; se mudou, reescreve esses campos e grava `dna_anterior`, `regenerada_em`, `regeneracoes`; repetição igual não escreve. (`app/ponto/portas/celula.py:277`; `app/ponto/portas/celula.py:285`; `app/ponto/portas/celula.py:293`) |
| **K** | Executa **R**, refaz inclusive `dna.lampadas`, mas conserva ata, veredito, impressão e carimbo anteriores; julgamento de futura é pulado. (`app/ponto/management/commands/gerar_celulas.py:94`; `app/ponto/portas/celula.py:298`; `app/ponto/portas/celula.py:453`) |
| **Z** | Fora da vigência, sendo gerada pela EC encerrada, sem lavra e sem outro vínculo cobrindo: põe `trabalha=False`, geradora nula, DNA sem marcos e marcas de regeneração. (`app/ponto/portas/celula.py:244`; `app/ponto/portas/celula.py:249`) |
| **N** | Regeneração direta cria a célula ausente em dia coberto. (`app/ponto/portas/celula.py:280`) |
| **B** | EC inativa sem fim: regeneração recusa e conserva células; vigia usa `vinculo_do_dia` e separa o caso em C6. (`app/ponto/portas/celula.py:146`; `app/ponto/services/vigia_cadastro_futuro.py:31`; `app/ponto/services/vigia_cadastro_futuro.py:40`) |
| **G** | Gerador cria ausente; conserva futura existente e editada; no passado pode preencher DNA nulo. (`app/ponto/portas/celula.py:875`; `app/ponto/portas/celula.py:881`; `app/ponto/portas/celula.py:883`; `app/ponto/portas/celula.py:892`) |

O suplemento usa **J = união das competências correntes das empresas ativas**, limitada inferiormente por amanhã e por `desde`; chama a regeneração para cada data existente, com `estender_horizonte=False`. Isso é a janela do gerador, não uma janela calculada individualmente por empresa dentro do suplemento. (`app/ponto/management/commands/gerar_celulas.py:281`; `app/ponto/services/cadastro_futuro.py:16`; `app/ponto/services/cadastro_futuro.py:30`)

| Evento e entrada efetiva | Futura não existe | Futura gerada | Futura editada | Futura carimbada, origem gerada | Futura regenerada |
|---|---|---|---|---|---|
| **Salvar estrutura: município do posto**. Evento somente se mudou município; percorre vínculos do posto. (`app/colaboradores/services/estrutura.py:53`; `app/colaboradores/services/vinculo.py:931`) | I / I / = | R / R / =, ressalva **D2** | I / I / = | K / coerência / **D1** | R / R / =, ressalva **D2** |
| **Salvar demais campos de estrutura**: essas operações apenas salvam; não chamam o suplemento. (`app/colaboradores/services/estrutura.py:18`; `app/colaboradores/services/estrutura.py:36`; `app/colaboradores/services/estrutura.py:75`; `app/colaboradores/services/estrutura.py:82`) | I / depende do efeito / ? | I / R se muda previsto / ? | I / I / = | I / depende do efeito / ? | I / R se muda previsto / ? |
| **Salvar template pela porta comum**: futuro percorre todos os vínculos do tipo, inclusive encerrados. (`app/escala/services/cadastro_tipo.py:368`) | I / I / = | R / R / =, **D2** | I / I / = | K / coerência / **D1** | R / R / =, **D2** |
| **Salvar template pelo admin**: salva e propaga somente futuras no mesmo atomic. (`app/escala/admin.py:11`; `app/escala/services/cadastro_tipo.py:380`) | I / I / = | R / R / =, **D2** | I / I / = | K / coerência / **D1** | R / R / =, **D2** |
| **Folga lançada/alterada/foto relavrada**: passado pela regeneração; futuro pelo suplemento. (`app/escala/folgas.py:247`; `app/escala/folgas.py:290`; `app/escala/folgas.py:298`) | I / I / = | R / R / = | I / I / = | K / coerência / **D1** | R / R / = |
| **Salvar calendário / trocar calendário do vínculo**: sincroniza foto futura e depois células existentes. (`app/escala/folgas.py:597`; `app/escala/folgas.py:619`; `app/escala/folgas.py:638`; `app/colaboradores/services/vinculo.py:455`) | I / I / = | R / R / = | I na célula / I / = | K / coerência / **D1** | R / R / = |
| **Vínculo novo pelo executor**: cria EC e suplementa desde início. (`app/colaboradores/services/vinculo.py:281`; `app/colaboradores/services/vinculo.py:330`) | I / I / = | R pelo novo / R / = | I / I / = | K pelo novo / coerência / **D1** | R pelo novo / R / = |
| **Encerrar com sucessor no executor**: fecha anterior em D−1 e depois aplica novo desde D. (`app/colaboradores/services/vinculo.py:205`; `app/colaboradores/services/vinculo.py:333`) | I / I / = | Z transitório ou conserva; termina R / R / = | I / I / = | termina K / coerência / **D1** | termina R / R / = |
| **Encerrar sem sucessor por `fechar_vigencia`**: começa após menor fim antigo/novo. (`app/colaboradores/services/vinculo.py:640`; `app/colaboradores/services/vinculo.py:648`) | I / I / = | Z / sem previsão inválida / **D3** | I / I / = | Z, ata conservada / coerência / **D1+D3** | Z / sem previsão inválida / **D3** |
| **Encerrar por `encerrar_vinculo`**: `save` dispara signal de vigência. (`app/colaboradores/services/vinculo.py:821`; `app/escala/signals.py:80`) | I / I / = | Z sem outro cobrindo; conserva havendo outro / evento deve fechar efeito / **D4** | I / I / = | mesma ação + ata antiga / coerência / **D1+D4** | mesma regra / evento deve fechar efeito / **D4** |
| **Reativar pelo executor**: mesma EC recebe ativa, fim nulo e suplemento. (`app/colaboradores/services/vinculo.py:253`; `app/colaboradores/services/vinculo.py:270`; `app/colaboradores/services/vinculo.py:333`) | I / I / = | R / R / = | I / I / = | K / coerência / **D1** | R / R / = |
| **Reabrir vigência impossível**: `update`, sem suplemento. (`app/colaboradores/services/vinculo.py:715`; `app/colaboradores/services/vinculo.py:730`) | I / I / = | I / R / **D5** | I / I / = | I / atualizar efeito / **D5** | I / R / **D5** |
| **Trocar tipo/configuração pelo executor ou salvar template**: segue as respectivas linhas acima; âncora alterada para valor não nulo tem signal próprio. (`app/colaboradores/services/vinculo.py:268`; `app/escala/signals.py:111`; `app/escala/signals.py:131`) | I / I / = | R / R / = | I / I / = | K / coerência / **D1** | R / R / = |
| **Trabalha em feriado**: flag mudou → suplemento; valor igual retorna. (`app/colaboradores/services/vinculo.py:869`; `app/colaboradores/services/vinculo.py:888`) | I / I / = | R / R / = | I / I / = | K / coerência / **D1** | R / R / = |
| **Feriado salvo/movido**: datas antiga/nova; somente geradas; EC vem do juiz alimentado pela geradora. (`app/core/signals.py:14`; `app/core/signals.py:27`; `app/ponto/services/cadastro_futuro.py:53`; `app/ponto/services/cadastro_futuro.py:59`) | I / I / = | R se juiz responde; B no caso C6 / mesma autoridade / = | I / I / = | K se juiz responde / coerência / **D1** | R ou B / mesma autoridade / = |
| **Feriado apagado**: mesma seleção, na data apagada. (`app/core/signals.py:30`; `app/ponto/services/cadastro_futuro.py:49`) | I / I / = | R ou B / mesma autoridade / = | I / I / = | K / coerência / **D1** | R ou B / mesma autoridade / = |
| **DnaContrato**: cenário composto de encerramento, sucessor e edição de template; não é porta de cadastro autônoma. (`app/escala/tests/test_celula_dia.py:146`; `app/escala/tests/test_celula_dia.py:152`; `app/escala/tests/test_celula_dia.py:165`; `app/escala/tests/test_celula_dia.py:186`) | segue portas acima | segue portas acima | segue portas acima | cenário não prova esta coluna / exigência pendente / **D1** | segue portas acima |
| **`regenerar_celulas_vinculo` direta**: intervalo explícito; extensão ligada por padrão. (`app/ponto/portas/celula.py:110`; `app/ponto/portas/celula.py:187`) | N / suplemento deveria I / **D6 se usada como suplemento** | R ou Z ou B / conforme ato / ressalvas acima | I / I / = | K / coerência / **D1** | R ou Z ou B / conforme ato / ressalvas acima |
| **Cron `gerar_celulas`**: mede antes, trata hoje separadamente, gera ausentes e conserva futuras existentes. (`app/ponto/management/commands/gerar_celulas.py:337`; `app/ponto/management/commands/gerar_celulas.py:340`) | G só EC ativa / gerar ausentes vigentes / **D7** | I + vigia / I + vigia / = | I / I / = | I + vigia de DNA / vigiar coerência / **D1** | I + vigia / I + vigia / = |

As ressalvas da matriz são estas:

- **D1 — desenho novo com julgamento antigo.** `dna.lampadas` é previsão; `ata.lampadas` é o fato lavrado. A regeneração futura muda o primeiro e conserva o segundo. O leitor usa as lâmpadas da ata, e há consumidor que casa marco do DNA com luz da ata. O vigia compara `trabalha`/DNA, sem conferir essa coerência. (`app/ponto/portas/celula.py:298`; `app/ponto/portas/celula.py:453`; `app/escala/services/leitor_celula.py:114`; `app/chamados/juizes.py:1337`; `app/ponto/services/vigia_cadastro_futuro.py:48`)
- **D2 — evento de atributo pode substituir o dono do dia.** O suplemento seleciona todas as futuras geradas da pessoa, sem restringir geradora; cada EC que cobre a data pode substituir o carimbo. Na propagação do template, vínculos são percorridos por início decrescente; havendo sobreposição do mesmo template, o mais antigo pode escrever por último, embora `vinculo_do_dia` preservasse o carimbo recebido. É um contraexemplo estático; ocorrência em produção **[nao verificado]**. (`app/ponto/services/cadastro_futuro.py:22`; `app/ponto/services/cadastro_futuro.py:26`; `app/escala/services/cadastro_tipo.py:373`; `app/escala/models.py:1003`; `app/ponto/portas/celula.py:294`; `app/escala/alimentacao.py:279`)
- **D3 — fim sem sucessor ganhou representação, mas zero não comprova sua semântica.** A FK fica nula; `dna.vinculo_id` continua sendo a EC encerrada; o vigia põe o caso em `sem_geradora`, fora dos dois contadores esperado-zero. O teste exige essa representação, mas o aval transcrito não a especifica literalmente. (`app/ponto/portas/celula.py:250`; `app/ponto/management/commands/gerar_celulas.py:97`; `app/ponto/services/vigia_cadastro_futuro.py:34`; `app/ponto/tests/test_cadastro_futuro_c4.py:60`) **lei: representação e leitura da célula futura existente cujo vínculo terminou sem sucessor. [nao verificado]**
- **D4 — encerrar não equivale sempre a aplicar sucessor.** Havendo outro vínculo cobrindo, a porta conserva a célula fora da vigência da geradora antiga; só o evento que efetivamente aplica o sucessor troca o carimbo. Encerramento isolado não completa essa transição. (`app/ponto/portas/celula.py:247`; `app/colaboradores/services/vinculo.py:333`)
- **D5 — reativação por `update` fica fora do evento.** `reabrir_vigencia_impossivel` atualiza ativa/fim, registra trilha e retorna; não chama regeneração nem emite `save`. Introdução dessa omissão pela c4b **[nao verificado]**. (`app/colaboradores/services/vinculo.py:715`; `app/colaboradores/services/vinculo.py:722`; `app/colaboradores/services/vinculo.py:730`)
- **D6 — regeneração explícita e suplemento têm contratos distintos.** A primeira cria ausentes e estende horizonte; o segundo só visita existentes. ESCALA-AUTO ainda usa diretamente a primeira, inclusive com horizonte padrão. O alcance anterior autorizado desse ato não pode ser revogado por inferência desta obra. (`app/ponto/portas/celula.py:187`; `app/ponto/portas/celula.py:280`; `app/escala/services/escala_auto_executor.py:99`; `app/escala/services/escala_auto_executor.py:104`)
- **D7 — ausente com EC inativa ainda vigente.** O cron enumera apenas `ativa=True`; a autoridade admite inativa com fim declarado cobrindo a data. Assim, uma lacuna anterior a um fim futuro pode ficar fora do suplemento e do gerador. É consequência das seleções; existência em produção **[nao verificado]**. (`app/ponto/services/cadastro_futuro.py:22`; `app/ponto/portas/celula.py:851`; `app/escala/alimentacao.py:214`)

Para o **passado**, estas são as expansões por estado:

| Tratamento | Não existe | Gerada | Editada | Carimbada | Regenerada |
|---|---|---|---|---|---|
| **P0 — fora do alcance do evento** | conserva ausência | conserva | conserva | conserva | conserva |
| **PR — chamada formal de regeneração** | cria se coberta | R; fora da vigência pode apagar trabalho/marcos mantendo geradora | conserva | desenho pode mudar; julgamento depende de `recalculo` e proteção humana | R, com nova marca somente se mudou |
| **PG — gerador** | **cria: PROIBIDO** | DNA nulo pode ser preenchido: **PROIBIDO** | conserva | mesma regra de DNA nulo: **PROIBIDO** se escrever | conserva DNA existente |
| **PX — exclusão de EC referenciada** | sem efeito | FK pode virar nula: **PROIBIDO** | restauração recusa | FK pode virar nula: **PROIBIDO** | FK pode virar nula: **PROIBIDO** |

**PR** vem dos ramos de criação, reescrita e fora de vigência; o julgamento só ocorre com `recalculo`, excluindo vereditos humanos. **PG** escreve fora de `regenerar_celulas_vinculo`. **PX** decorre do `delete` e do `SET_NULL`, não de regeneração. (`app/ponto/portas/celula.py:260`; `app/ponto/portas/celula.py:280`; `app/ponto/portas/celula.py:285`; `app/ponto/portas/celula.py:444`; `app/ponto/portas/celula.py:875`; `app/ponto/portas/celula.py:886`; `app/colaboradores/services/vinculo.py:1005`; `app/colaboradores/services/vinculo.py:1007`; `app/escala/models.py:1693`)

| Evento | Passada: atual / aval / divergência |
|---|---|
| Estrutura/município | **P0 / P0 / =**; município trata hoje separadamente. (`app/colaboradores/services/vinculo.py:933`) |
| Template comum | **PR**, ativos, desde competência anterior/início da EC até hoje / preservar alcance anterior / anterioridade exata **[nao verificado]**. (`app/escala/services/cadastro_tipo.py:337`; `app/escala/services/cadastro_tipo.py:351`) |
| Template no admin | **P0 / P0 / =**. (`app/escala/services/cadastro_tipo.py:380`) |
| Folga/foto | **PR / alcance anterior pela mesma porta / = quanto ao escritor**. (`app/escala/folgas.py:290`; `app/escala/folgas.py:293`) |
| Calendário salvo/trocado | **P0 / P0 / =** no suplemento; foto reconciliada somente após hoje. (`app/escala/folgas.py:632`; `app/escala/folgas.py:638`) |
| Vínculo novo/reativado pelo executor | **PR**, início..hoje / preservar alcance anterior / anterioridade exata **[nao verificado]**. (`app/colaboradores/services/vinculo.py:319`) |
| Fechar por `fechar_vigencia`, com/sem sucessor | **P0 / P0 / =**. (`app/colaboradores/services/vinculo.py:640`) |
| Encerrar por `save` | **PR**, menor início..hoje / mesma porta / = quanto ao escritor. (`app/escala/signals.py:60`; `app/escala/signals.py:73`) |
| Reabrir vigência impossível | **P0 / não ampliar passado / =**; a lacuna é futura, D5. (`app/colaboradores/services/vinculo.py:715`) |
| Âncora para valor não nulo | **PR**, competência corrente..hoje / mesma porta / = quanto ao escritor. (`app/escala/signals.py:119`; `app/escala/signals.py:125`) |
| Trabalha em feriado | **PR**, competência anterior..hoje / mesma porta / = quanto ao escritor. (`app/colaboradores/services/vinculo.py:878`; `app/colaboradores/services/vinculo.py:883`) |
| Feriado salvo/apagado | **P0 / P0 / =** no suplemento. (`app/ponto/services/cadastro_futuro.py:49`) |
| DnaContrato | Verifica congelamento do histórico encerrado após salvar template / conservar passado / cenário presente; execução **[nao verificado]**. (`app/escala/tests/test_celula_dia.py:166`; `app/escala/tests/test_celula_dia.py:197`) |
| Regeneração explícita | **PR / exceção formal / = quanto ao escritor**, respeitadas lavra e edição humana. (`app/ponto/portas/celula.py:114`; `app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:285`) |
| Cron | **PG / nenhuma escrita passada fora da regeneração / PROIBIDO** pela regra desta leitura. (`app/ponto/portas/celula.py:875`; `app/ponto/portas/celula.py:886`) |
| Restaurar snapshot/desfazer ajuste | **PX / conservar passada / PROIBIDO quando houver referência passada**; ocorrência real **[nao verificado]**. (`app/colaboradores/services/vinculo.py:1007`; `app/colaboradores/services/vinculo.py:1077`; `app/colaboradores/services/vinculo.py:1079`; `app/escala/models.py:1693`) |

**2. As 11 falhas da c4**

A numeração segue o anexo. O anexo demonstra os resultados em c4; aprovação posterior de cada teste em c4b permanece **[nao verificado]**. [nao verificado]

| # | Falha / classe revelada / casa da matriz | Tratamento presente em c4b |
|---|---|---|
| 1 | `Cancelar_na_caixa...`: Feriado salvo × futura; data do evento sem representação canônica. O traceback fornecido morre no preparo, antes do clique. **[nao verificado]** | **Adaptação local por `isinstance`**: converte string ISO antes de comparar datas. Resolve essa incompatibilidade no serviço; não normaliza a origem `Feriado`. (`app/ponto/services/cadastro_futuro.py:48`; `app/core/models.py:12`) |
| 2 | `a_tela_de_feriado_novo...`: mesma classe e mesma casa; falha no preparo fornecido. **[nao verificado]** | Mesmo contorno; há teste específico enviando string ao evento. (`app/ponto/services/cadastro_futuro.py:48`; `app/ponto/tests/test_cadastro_futuro_c4.py:146`) |
| 3 | `Excluir_confirmado...`: mesma classe; não constitui prova do clique após preparar o cenário. **[nao verificado]** | Mesma conversão. Exclusão também passa `instance.data` ao serviço. (`app/ponto/services/cadastro_futuro.py:48`; `app/core/signals.py:30`) |
| 4 | `Salvar_do_editar...`: Feriado salvo/movido × futura nas datas antiga/nova. | Mesma conversão seletiva; captura da data antiga continua no `pre_save`. (`app/ponto/services/cadastro_futuro.py:48`; `app/core/signals.py:14`; `app/core/signals.py:27`) |
| 5 | `dna_congelado_sobrevive...`: encerramento → sucessor → template × futura gerada/regenerada; preservação histórica. | **Mudança da testemunha do teste.** Agora exige retirada do carimbo encerrado, aplica explicitamente o sucessor e fabrica divergência por `QuerySet.update` do template; depois verifica cura por `salvar_tipo`. Não é `skip`/allowlist, mas também não é uma cura de produção dessa falha isoladamente. (`app/escala/tests/test_celula_dia.py:159`; `app/escala/tests/test_celula_dia.py:165`; `app/escala/tests/test_celula_dia.py:170`; `app/escala/tests/test_celula_dia.py:177`; `app/escala/tests/test_celula_dia.py:186`) |
| 6 | `template_derivado_trilha_e_passado_intocado`: fechamento com sucessor × futura anterior ao início do efeito, ainda coberta. | **Cura na origem do alcance**: começa em `min(fim_antigo,fim_novo)+1`. O teste original conserva a comparação da foto ant

[truncado em 20000 de 37456 caracteres]

_4333 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
