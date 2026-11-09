# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 01:10** · publicado em **09/11/2026 01:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 01:10 · sessao 62a6fe9645f5 · E1-FILA-V2-2-TRAVA · censo · origem=Code

**O conjunto inicial não cobre todo o alcance possível do ato.** Há caminhos para travar/escrever chamados terminais que não eram órfãos nem destinos previamente coletados, e para alcançar perguntas por relações sem filtro de colaborador. Isso é demonstrável no código; ocorrência concreta, quantidade e colaboradores afetados em produção: **[nao verificado]**. (`chamados/reconciliador.py:910–954`, `:1582–1644`; `ponto/services/cluster_espurio.py:127–147`; `chamados/services/disputa_emissao.py:1747–1751`)

As referências abaixo são relativas a `app/`. O anexo manda consultar o código vivo quando houver divergência. (`docs/JUIZES.md:14`)

**1. Escritores e travas depois de `_trava_do_repasse`**

Chamando o colaborador do ato de `c` e os órfãos recebidos de `O`, o conjunto efetivamente travado hoje é:

| Classe | Predicado inicial |
|---|---|
| Colaborador | Somente `pk=c`. (`chamados/services/disputa_emissao.py:954`, `:958`) |
| Pergunta | `colaborador_id=c OR chamado_destino_id IN O`, sem restrição de dia, disputa ou materialização. (`chamados/services/disputa_emissao.py:956–959`) |
| Disputa | Disputas dessas perguntas, abertas ou fechadas, mais todas as abertas de `c`. (`chamados/services/disputa_emissao.py:958`; `chamados/models.py:2679–2684`) |
| Chamado | `O`, destinos dessas perguntas, todos os vivos de `c` e pais das disputas travadas **que estejam abertas**. Uma disputa fechada explicitada não acrescenta automaticamente seu pai. (`chamados/services/disputa_emissao.py:959`; `chamados/models.py:2685–2696`) |

A autoridade ordena cada tabela por PK, mas não verifica se os IDs recebidos pertencem ao colaborador informado. (`chamados/models.py:2674`, `:2683`, `:2696`, `:2700–2701`)

O censo do corpo e das cascatas síncronas é:

| Linha/modelo alcançado | Cadeia e operação | Está no conjunto inicial? |
|---|---|---|
| **Disputa da pergunta já costurada ao órfão** | Busca por destino → complementa perguntas → grava `motivos`. (`chamados/services/disputa_emissao.py:1002–1016`) | A disputa encontrada entra pela mesma relação de destino usada no topo. Seu colaborador não é acrescentado por isso. (`chamados/services/disputa_emissao.py:956–959`) |
| **Disputa aberta de `c`** | Busca aberta → grava motivos; também pode gravar contexto na cauda de entradas pendentes. (`chamados/services/disputa_emissao.py:1028–1030`, `:1063–1064`, `:1186–1202`, `:293–296`) | Sim, se preexistente e aberta no topo. (`chamados/models.py:2681–2684`) |
| **Disputa nova e chamado-pai novo/reutilizado** | `Chamado.abrir` → eventual nota/fusão de contexto → `Disputa.objects.create`. (`chamados/services/disputa_emissao.py:1036–1060`) | Pai reutilizado vivo de `c`: sim. Linhas novas ainda não existiam para serem travadas. (`chamados/models.py:2691`, `:971`; `chamados/services/disputa_emissao.py:1055`) |
| **Órfãos recebidos** | `analisar()` nos ramos complemento, intervalo, ata, episódio e fallback → `_gravar_estado` → `save`. (`chamados/services/disputa_emissao.py:1017`, `:1091`, `:1110`, `:1155`, `:1184`; `chamados/models.py:800–806`, `:290–296`) | Seus IDs estão explicitados no topo. (`chamados/services/disputa_emissao.py:955`, `:959`) |
| **Pergunta nova ou canônica reutilizada** | `_criar_pergunta_se_couber`/`gerar_perguntas_disputa` → `Pergunta.abrir` → `_nascer`; insere nova ou grava célula, destino e lavra da existente. (`chamados/services/disputa_emissao.py:834–843`, `:439–452`; `chamados/models.py:2985–3019`) | Existente com coluna `colaborador_id=c`: sim. Nascimento com disputa de outro dono pode tomar outro Colaborador depois do topo. (`chamados/services/disputa_emissao.py:956`; `chamados/models.py:2853`) |
| **Pergunta reutilizada com dia incorreto; sua disputa e destinos** | `_nascer` → trava C/D/Ch/P → `rechavear_no_reuso` → `rechavear_data_evento` → grava dia/célula/lavra; acorda célula antiga. (`chamados/models.py:2948–2965`; `chamados/services/fio.py:1001–1003`, `:1033–1039`) | Cobertos quando a pergunta entrou no topo: este já inclui sua disputa e destino atual; o destino pedido é órfão/vivo. Isso não cobre automaticamente donos divergentes. (`chamados/services/disputa_emissao.py:956–959`) |
| **Pergunta órfã criada pela cauda** | `perguntar_entradas_pendentes` → `gerar_perguntas_disputa` → `reatar_pergunta_orfa`. (`chamados/services/disputa_emissao.py:294`, `:459–464`, `:1202`) | A pergunta é nova; os chamados que o reatamento descobrir não estão necessariamente no topo. (`chamados/reconciliador.py:1582–1585`, `:910–921`) |
| **Destino vivo derivado; pergunta reapontada** | Derivador busca vivos do dono da disputa → `reapontar_resolvedora` → trava C/Ch, eventualmente D/P da colisão → grava pergunta. (`chamados/services/disputa_emissao.py:198–201`; `chamados/reconciliador.py:1552–1553`; `chamados/services/fio.py:889–925`) | Vivo de `c`: sim. Se o dono da disputa divergir de `c`, a cobertura não decorre do topo. (`chamados/models.py:2691`; `chamados/services/disputa_emissao.py:958`) |
| **Destino histórico encerrado** | Reatamento busca histórico → costura no histórico único → chama resolver. A costura trava o chamado mesmo sem alterar seu status. (`chamados/reconciliador.py:1582–1585`, `:1638–1644`; `chamados/services/fio.py:768`) | **Não necessariamente:** pode não ser órfão, vivo, pai aberto nem destino anterior de pergunta coletada. (`chamados/services/disputa_emissao.py:956–959`; `chamados/models.py:2687–2691`) |
| **Chamado do dia renascido pelo c2** | Reatamento sem histórico → c1 ou c2 → `abrir(colaborador, módulo, data)` sem status → renascimento → costura. (`chamados/reconciliador.py:1615–1618`, `:910–954`; `chamados/models.py:906`, `:921–923`) | **Não necessariamente**, pela mesma ausência dos terminais independentes no predicado inicial. (`chamados/models.py:2687–2691`) |
| **Disputa fechada pelo próprio agrupador/c1** | `pode_fechar` → `fechar` → trava universo C/D/Ch → grava fechamento. (`chamados/services/disputa_emissao.py:1210–1219`; `chamados/reconciliador.py:405–413`; `chamados/models.py:1918–1920`, `:1940–1944`) | Disputa aberta preexistente de `c`: sim; disputas novas não existiam no topo. Pais/relações de outro dono exigem análise relacional adicional. (`chamados/models.py:2681–2687`) |
| **Pai pelo S88** | `post_save(Disputa)` → busca pai só por PK → `reconciliar` → nota. O gate pode travar novamente C, abertas e chamados. (`chamados/signals.py:18–45`; `chamados/models.py:711`) | Pai de disputa aberta no topo: sim. FK para outro dono ou disputa que só abriu posteriormente: cobertura não garantida. (`chamados/models.py:2685–2691`) |
| **Perguntas pelo S143/S128** | Chamado encerrado → resolver → busca por destino → carimba P → pode reabrir pai, carimbar irmãs e fechar disputas. (`chamados/signals.py:90–115`; `chamados/services/disputa_emissao.py:1721`, `:1747–1751`, `:1787`, `:1804–1814`, `:1827–1888`) | P de `c`: sim. P estrangeira ligada a chamado alcançado fora de `O`, suas irmãs e sua disputa: **não necessariamente**. (`chamados/services/disputa_emissao.py:956`, `:1747–1751`, `:1836–1861`) |
| **Perguntas pelo S144** | `post_save(Disputa fechada)` → todas as pendentes daquela disputa → `carimbar` → `save`. Esse receiver não planta batida. (`chamados/signals.py:147–163`; `chamados/services/materializacao.py:1408–1431`; `chamados/models.py:3152`) | Sim para as P coletadas pelo topo; não há filtro pela coluna colaborador na consulta do S144. (`chamados/services/materializacao.py:1418–1419`) |
| **Perguntas relavradas pela mudança de estado** | `_gravar_estado`, cruzando a fronteira de `ENCERRADOS`, busca P por destino sem dono → `lavrar` → salva lavra. (`chamados/models.py:273`, `:303–312`; `chamados/veredito_lavrado.py:234–255`) | P de `c`: sim. P estrangeira em chamado alcançado fora de `O`: **não necessariamente**. (`chamados/services/disputa_emissao.py:956–959`) |
| **Urgência do próprio chamado** | `post_save(Chamado encerrado)` → `_rebaixar_sem_post_save`. (`chamados/signals.py:53–72`) | A cobertura é a do próprio chamado; não amplia por si só o conjunto de chamados. (`chamados/signals.py:72`) |

O cartório amplia o alcance acima:

| Caminho do cartório | Escrita/trava e cobertura |
|---|---|
| **Entrada síncrona** | Nascimento/mudança de status → `julgar_celula(colaborador, dia)` → julgamento da célula. (`chamados/signals.py:187–203`; `ponto/services/cartorio.py:1976–1991`) |
| **E2, colaborador inteiro** | Busca P por `disputa__colaborador`, inclusive disputas fechadas → carimba → eventualmente fecha disputa. Se a coluna P.colaborador divergir da disputa, o topo pode não ter coletado a P. (`ponto/services/cartorio.py:958–963`; `chamados/reconciliador.py:593–614`, `:627–641`; `chamados/services/disputa_emissao.py:956`) |
| **Fecho e reatamento dos vivos** | Reconcilia chamados vivos, podendo passar novamente por S143 e pela costura anterior ao fechamento. Vivos de `c` estão no topo; suas perguntas estrangeiras não são coletadas automaticamente. (`ponto/services/cartorio.py:1488`, `:1501`, `:1522`; `chamados/models.py:435–437`, `:2691`) |
| **Ressuscitação de perguntas** | Chamado → relação reversa `perguntas_que_resolvem`, sem dono → trava suas D/P → registra decisões antigas → descarimba em lote → reabre disputas fechadas. A guarda do lote pode recusar dono não travado; ela não demonstra cobertura do conjunto inicial. (`chamados/models.py:1483–1489`, `:1550–1572`, `:3258–3266`) |
| **Pai de disputa reaberta** | Depois da ressuscitação, uma nova chamada com `abertas_do_colaborador=True` passa a incluir o pai dessa disputa, mesmo terminal. Ele podia faltar no topo quando a disputa estava fechada. (`chamados/models.py:1571–1572`, `:2685–2687`; `chamados/services/disputa_emissao.py:530`) |
| **Fábrica após lavra** | Desfaz carimbos, trava C/D/P, cria/agrega disputa e gera perguntas. No julgamento, o primeiro descarimbo usa `reabrir_fechada=False`; o helper `perguntar` também chama o descarimbador por motivo. (`ponto/services/cartorio.py:985–997`; `chamados/services/fabrica_da_celula.py:145–151`, `:174–186`; `chamados/services/relavra_via.py:230–246`) |
| **Emissor do chamado do dia** | `abrir` por colaborador/módulo/data **sem filtro de status**; pode renascer terminal não coletado e depois atualizar seus marcos/contexto. (`ponto/services/cartorio.py:1740–1742`, `:1778–1789`; `ponto/management/commands/detectar_ausencias.py:693–713`, `:743–744`) |
| **Cluster de outros dias** | Julgamento → cluster → janela de 14 dias → `abrir` sem status; fecha vivos ou tenta renascer encerrados daquelas chaves. Terminais independentes das relações iniciais faltam no topo. (`ponto/services/cartorio.py:990–994`; `ponto/services/cluster_espurio.py:12`, `:40–49`, `:123–147`) |
| **Limbo** | Emissor → `agrupar_furo` → trava C/D abertas/Ch furo+pai → contexto, reapontamento, reconciliação e nota. (`ponto/management/commands/detectar_ausencias.py:760–761`; `chamados/services/limbo_folgas.py:274–292`) |

Há ainda escritas em modelos fora das quatro tabelas da autoridade:

| Modelo | Onde é escrito; cobertura pelo topo |
|---|---|
| `CelulaDia` | Lavra/carimbo/precedência do cartório → `save`; **não está no topo**. (`ponto/services/cartorio.py:1802`, `:1822`, `:1997`; `ponto/portas/celula.py:900`, `:1067`, `:1082`) |
| `EventoCelula` | INSERT pelo save da célula, nascimento de chamado e mudanças de pergunta; **não está no topo**. Emitir o evento não equivale a consumir o reconciliador naquela chamada. (`ponto/portas/celula.py:882–902`; `chamados/signals.py:237–244`; `chamados/models.py:3239–3248`) |
| `DecisaoPergunta` | Ressuscitação preserva a projeção anterior antes do descarimbo; **não está no topo**. (`chamados/models.py:1563–1565`) |
| `FollowupChamado` | Notas de agrupamento, renascimento, fechamento e reabertura → INSERT. Só há escrita do Chamado pela nota quando existe `contexto_novo`. (`chamados/services/nota.py:102–123`) |
| `LogAuditoria` | Eventos/trilhas convergem ao INSERT; **não está no topo**. (`core/observ.py:134–137`) |
| `User` sistema | `get_user_sistema` pode inserir por `get_or_create`; **não está no topo**. (`core/utils.py:18`) |
| `Pauta` e `ContadorPauta` | Limbo exportado → pauta → contador departamental, com criação/UPDATE; **não estão no topo**. (`chamados/services/fabrica_da_celula.py:181–182`, `:245–251`; `pautas/services.py:180–188`, `:352–357`) |
| `PushSubscription` | Push pode apagar inscrição expirada; **não está no topo**. (`chamados/push_utils.py:24`, `:49–51`) |
| `Colaborador`, campos FCM | Push pode limpar token em lote, inclusive de outro colaborador; cobertura não garantida. (`chamados/fcm_utils.py:41`, `:89–94`; `colaboradores/fcm.py:60–62`; `colaboradores/services/colaborador.py:88`) |

Os callbacks precisam de uma fronteira separada: são agendados por `on_commit`, portanto as travas da transação original não protegem suas transações posteriores. (`chamados/signals.py:144`, `:225`, `:293`, `:356`)

| Callback | Alcance posterior |
|---|---|
| **Beco da pergunta** | Reabre uma transação, trava universo D/Ch, resolve destinos terminais e fecha D. (`chamados/signals.py:214–225`; `chamados/services/becos.py:41–57`, `:72–78`) |
| **Isento** | Percorre todos os vivos do colaborador e escreve o véu de silêncio, com trava/save por chamado. (`chamados/signals.py:269–293`; `chamados/services/silencio_isentos.py:24–31`, `:41–58`; `chamados/services/arquivo.py:170–173`) |
| **Filho morto** | Chama o reconciliador completo: E2, reatamento e retratação dos vivos. A retratação também pode rejeitar a `Justificativa` relacionada. (`chamados/signals.py:337–356`; `chamados/reconciliador.py:1428–1437`, `:1449–1460`, `:529–540`) |
| **Regularizações confirmadas** | Pode gravar `RegularizacaoItemBatida`/`RegularizacaoExterna`, criar/corrigir/apagar Batida e encerrar seu chamado. Batida dispara novamente núcleo, cartório e reconciliador. (`chamados/signals.py:129–144`; `chamados/services/regularizacao_ext.py:127–205`, `:218–224`; `ponto/signals.py:26–50`, `:135–140`, `:215`) |
| **Derivações da Batida retomada** | Reconstrói `TurnoMaterializado`, emite eventos e agenda recalcular fechamento; este grava `FechamentoMensal` e substitui `DiaPago`. Outros efeitos internos da derivação financeira: **[nao verificado]**. (`ponto/nucleo.py:59–93`; `ponto/services/fechamento.py:898`, `:1144`, `:1160`, `:1191–1201`, `:1810–1824`; `ponto/services/dia_pago.py:359–360`) |

**2. Dono: pode alcançar outro colaborador?**

**Sim, condicionalmente às relações e argumentos existentes.** Os caminhos abaixo não permitem concluir que isso ocorreu em produção: **[nao verificado]**.

| Acesso sem filtro suficiente de dono | Consequência possível |
|---|---|
| Complemento: `filter(chamado_destino=ch, materializada_em__isnull=True)` | Obtém a disputa pela pergunta encontrada, sem exigir que seja de `c`. O topo inclui essa D/P, mas não seu Colaborador. (`chamados/services/disputa_emissao.py:1002–1016`, `:956–959`) |
| S143: `filter(chamado_destino=chamado, …)` | Pode carimbar P estrangeira, alcançar sua disputa, irmãs e pai. (`chamados/services/disputa_emissao.py:1747–1751`, `:1787`, `:1804–1814`, `:1827–1861`) |
| Relavra: `filter(chamado_destino_id=self.pk, validada_em__isnull=True)` | Pode salvar lavra de P estrangeira ao mudar o status do chamado. (`chamados/models.py:309–312`; `chamados/veredito_lavrado.py:255`) |
| Ressuscitação: `self.perguntas_que_resolvem` | Pode alcançar D/P de outro dono. O lote tem guarda de colaboradores travados e pode recusar; não há filtro de proprietário na seleção reversa. (`chamados/models.py:1483–1489`, `:1550–1565`, `:3258–3260`) |
| S88: `filter(pk=instance.chamado_id)` | Pode entrar no gate do pai de outro colaborador se a FK estiver assim. (`chamados/signals.py:32–42`) |
| FCM: `Colaborador.objects.filter(fcm_token=fcm_token)` | Pode atualizar outros colaboradores com o mesmo token, sem filtro por `c`. (`chamados/fcm_utils.py:41`; `colaboradores/fcm.py:55–62`) |
| Regularização pós-commit: `item.batida_alvo` | A operação usa a Batida relacionada sem conferir ali o colaborador; uma relação cruzada pode disparar cascatas do outro dono. (`chamados/services/regularizacao_ext.py:127–155`) |

As portas atuais recusam **nova costura** para destino de outro dono, mas isso não valida universalmente relações já gravadas nem os argumentos da porta externa. (`chamados/models.py:2848`; `chamados/services/fio.py:863–865`, `:1575–1579`)

O próprio nascimento de P verifica igualdade entre sua coluna colaborador e a disputa; essa verificação só roda quando `_state.adding`. Integridade de todo o acervo existente: **[nao verificado]**. (`chamados/models.py:3420–3423`, `:3430–3435`)

**3. Chamado terminal pode voltar a ser escrito? Entra hoje?**

**Sim, especialmente `resolvido`/`fechado`.** Estes são `ENCERRADOS`; `cancelado`/`superado` também são terminais, mas o renascimento não os reabre. (`chamados/catalogo/motor.py:24–32`; `chamados/models.py:991–993`, `:1019`)

Há três cadeias independentes:

- **Reatar histórico:** cauda do agrupador → geração → reatamento → busca histórico encerrado → costura → trava do chamado → resolver. Aqui ele pode apenas ser travado, sem renascer. (`chamados/services/disputa_emissao.py:1202`, `:294`, `:459–464`; `chamados/reconciliador.py:1582–1585`, `:1638–1644`; `chamados/services/fio.py:768`)
- **Renascer chamado do dia:** reatamento → c2 → `abrir` sem filtro de status → `_renascer_por_premissa_viva` → `reabrir` → grava estado → ressuscita perguntas. (`chamados/reconciliador.py:1617`, `:910–921`; `chamados/models.py:921–923`, `:1019`, `:1442–1451`)
- **Renascer pela cascata do cartório:** mudança de status → cartório → emissor do dia ou cluster de outros dias → `abrir` sem status → mesmo renascimento. (`chamados/signals.py:203`; `ponto/services/cartorio.py:990–994`, `:1778–1780`; `ponto/management/commands/detectar_ausencias.py:693–713`; `ponto/services/cluster_espurio.py:130–147`)

**Ele só entra hoje se coincidir com alguma relação inicial:** órfão explícito, destino coletado ou pai de disputa aberta travada. O ramo “todos do colaborador” inclui somente `VIVOS`; não cobre todos os terminais dessas chaves. (`chamados/services/disputa_emissao.py:955–959`; `chamados/models.py:2685–2696`)

O filtro direto da fábrica que rejeita `ENCERRADOS` não elimina esses caminhos indiretos. (`chamados/services/disputa_emissao.py:755–756`; `chamados/reconciliador.py:910–921`)

**4. Fecho mínimo e computabilidade**

Para as **linhas preexistentes das quatro entidades**, o mínimo precisa ser um **fecho relacional dos caminhos efetivamente alcançáveis**, não apenas “abertas/vivos de `c`”. As relações necessárias são:

1. Colaboradores donos de cada Chamado, Disputa e Pergunta alcançada — considerando tanto a coluna da P quanto sua disputa, sem presumir igualdade no legado. (`chamados/models.py:2853`; `chamados/services/fio.py:889`, `:1001`; `chamados/reconciliador.py:628`)
2. Disputas das perguntas alcançadas e abertas dos colaboradores cujo universo for solicitado; também as fechadas que a ressuscitação pode reabrir. (`chamados/models.py:1562–1572`, `:2681–2684`)
3. Chamados recebidos, destinos, pais das disputas alcançadas **inclusive as inicialmente fechadas que podem abrir**, vivos dos universos solicitados e candidatos históricos/terminais dos lookups do reatamento, emissor e cluster. (`chamados/models.py:2685–2691`; `chamados/reconciliador.py:1582–1585`, `:910–921`; `ponto/services/cluster_espurio.py:130–147`)
4. Perguntas alcançadas por coluna colaborador, por dono da disputa, por destino reverso, por âncora no pai sem destino e por irmandade dentro de disputa; repetir as relações enquanto surgirem novos membros. (`chamados/services/disputa_emissao.py:956`, `:1747–1751`, `:1836`; `chamados/reconciliador.py:628`; `chamados/models.py:345`, `:1483–1489`)
5. Se a notificação fizer parte do ato, colaboradores selecionáveis pela mesma chave FCM, além dos modelos auxiliares efetivamente escritos. (`chamados/fcm_utils.py:41`; `colaboradores/fcm.py:60–62`)

Esse é o predicado de **menor fecho por a

[truncado em 20000 de 33469 caracteres]

## 09/11 01:04 · sessao 35a2cddeaac6 · DISPARAR-PERGUNTAS-NO-CONTRATO-20 · censo · origem=Code

## (1) Conceito de fio mudo e comparação com os juízes

**O comando usa os juízes de vitalidade existentes, mas seu universo emitível não é simplesmente “todo chamado vivo sem pergunta viva”.** Ele acrescenta janela, célula, motivo vencido, situação do colaborador e dedup por identidade do fato. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `app/chamados/management/commands/disparar_perguntas_competencia.py:154`, `app/chamados/management/commands/disparar_perguntas_competencia.py:215`)

| Pergunta | Autoridade registrada | Uso no comando |
|---|---|---|
| Chamado vivo? | `_C1 → VIVOS`. `VIVOS = aberto, em_analise, registrado`; `COBRANCA = aberto, em_analise`. (`app/core/juizes.py:261`; `app/chamados/catalogo/motor.py:24`) | `status_local__in=VIVOS`, com célula dentro de `ini/fim`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`) |
| Pergunta viva? | `_C6 → chamados.juizes.pergunta_viva`. (`app/core/juizes.py:266`) | Importa a reexportação de `models` e aplica a cada pergunta ligada por `chamado_destino`. (`app/chamados/models.py:4062`; `app/chamados/management/commands/disparar_perguntas_competencia.py:133`, `app/chamados/management/commands/disparar_perguntas_competencia.py:148`) |
| Alguma pergunta da coleção viva? | `_C15 → fio_vivo`. (`app/core/juizes.py:275`) | Repete `any(pergunta_viva(...))`, equivalente ao corpo do juiz, sobre a coleção escolhida pelo comando. (`app/chamados/juizes.py:1253`; `app/chamados/management/commands/disparar_perguntas_competencia.py:148`) |

O anexo também distingue os conjuntos do motor; o registro vivo explica expressamente que `VIVOS` responde existência do chamado, não existência de cobrança/fio. (`app/docs/JUIZES.md:54`; `app/core/juizes.py:100`)

O C6 não se reduz a `materializada_em IS NULL`: considera validação, tranca, vias de identidade/retratação, resposta textual, decisão administrativa, marco no espelho, célula provisória, falta e lastro. Uma materializada pode continuar viva; uma não materializada pode estar morta. (`app/chamados/juizes.py:1152`, `app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1189`, `app/chamados/juizes.py:1196`, `app/chamados/juizes.py:1216`, `app/chamados/juizes.py:1227`)

**DECISÕES encontradas na comparação:**

1. **Universo de chamados:** o comando exige célula/janela, inclui `registrado` e não restringe módulo. Isso é mais específico que `_C1` e mais largo que “cobrança de batida”. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/catalogo/motor.py:24`)

2. **Universo de perguntas:** o primeiro teste considera todas as perguntas destinadas àquele chamado, sem restringir motivo, dia ou `conversa=False`. Uma conversa viva também satisfaz esse teste; o dedup posterior do fato exclui conversas. (`app/chamados/management/commands/disparar_perguntas_competencia.py:147`; `app/chamados/models.py:2752`)

3. **Coleção diferente da do vigia:** o comando olha perguntas do chamado; `vigia_fio.censo` olha todas as perguntas da célula. Pergunta viva sob outro destino pode produzir respostas diferentes entre os leitores. (`app/chamados/management/commands/disparar_perguntas_competencia.py:148`; `app/chamados/services/vigia_fio.py:398`)

4. **Vitalidade versus identidade:** `_ja_perguntou` consulta motivo/data, sem conversa, dentro de uma disputa; `linhas_dos_fatos` consulta colaborador/motivo/dia/instância, independentemente de vitalidade. O comando usa ambas, nessa ordem. (`app/chamados/services/disputa_emissao.py:1316`; `app/chamados/models.py:2751`; `app/chamados/management/commands/disparar_perguntas_competencia.py:172`, `app/chamados/management/commands/disparar_perguntas_competencia.py:215`)

5. **Retratada/duplicata versus encerrada:** `linhas_dos_fatos` exclui `duplicata` e `motivo_retratado`; não exclui uma linha apenas por validada, materializada ou morta. Portanto o rótulo **FIO DECIDIDO** significa, nesse ponto, “existe linha do fato”; não comprova decisão humana. (`app/chamados/models.py:2391`, `app/chamados/models.py:2753`; `app/chamados/management/commands/disparar_perguntas_competencia.py:215`, `app/chamados/management/commands/disparar_perguntas_competencia.py:225`)

6. **Guardas da fábrica:** o caminho célula consulta `dia_sem_trabalho`, futuro e veredito acusador; o fio mudo chama diretamente `motivos_pela_ata`, além de parcial/teto/ativo. O texto “dia ACUSADO pela lei” não corresponde a um teste explícito de `VEREDITOS_ACUSAM` nesse ramo. (`app/chamados/services/fabrica_da_celula.py:89`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`, `app/chamados/management/commands/disparar_perguntas_competencia.py:158`, `app/chamados/management/commands/disparar_perguntas_competencia.py:221`)

7. **Competência entregue no limbo:** o evento da célula manda `trabalha=None` em competência entregue à Pauta DP; o fio mudo chama diretamente `perguntar`, sem essa composição. (`app/chamados/services/fabrica_da_celula.py:181`; `app/chamados/management/commands/disparar_perguntas_competencia.py:282`)

8. **Tempo:** o fio usa dia do turno da célula e teto por instante, podendo incluir hoje; o vigia do ramo célula exclui hoje e ontem enquanto ontem pertence à janela do vigia de hora. Passado sem marcos continua autorizado por comparação de data. (`app/chamados/management/commands/disparar_perguntas_competencia.py:102`, `app/chamados/management/commands/disparar_perguntas_competencia.py:110`, `app/chamados/management/commands/disparar_perguntas_competencia.py:150`; `app/chamados/services/fabrica_da_celula.py:29`)

## (2) Escritores — inventário AST e leitura dos efeitos

A classificação abaixo distingue **escrita do carimbo**, **mudança de vitalidade C6** e **perda da pergunta daquele chamado por reapontamento**. O censo nativo identifica escritas por modelo, campos e relações; as chamadas às portas foram acrescentadas por AST e classificadas pela leitura dos corpos. Ocorrências e transições concretas em produção: **[nao verificado]**. (`app/core/censo_escritas.py:6`, `app/core/censo_escritas.py:20`, `app/core/censo_escritas.py:469`)

Nas tabelas, **C/D/Ch/P** significam Colaborador/Disputa/Chamado/Pergunta, a ordem declarada em `trava_na_ordem`. “Sem trava própria” não exclui trava herdada do chamador. (`app/chamados/models.py:2627`)

**Evento comum das perguntas:** os `save()` relevantes acionam outbox síncrono e beco `on_commit`; save e outbox compartilham atomic. O `save()` não adquire genericamente C nem exige seu registro. (`app/chamados/signals.py:214`, `app/chamados/signals.py:237`; `app/chamados/models.py:3403`)

### (2a) Portas que escrevem a pergunta sem escrever diretamente seu chamado

| Função/sítio | Escrita e transação/trava | Signal/evento atual |
|---|---|---|
| `PerguntaDisputa.carimbar` | Carimbo/via, opcionalmente resposta/validação/lavra; só grava se C6 aceitar a morte resultante. Sem trava própria; atomic do `save`. (`app/chamados/models.py:3093`, `app/chamados/models.py:3128`, `app/chamados/models.py:3142`) | Evento comum de P. (`app/chamados/signals.py:237`) |
| `carimbar_identidade` | Carimbo/via/recusa; não consulta C6; sem trava própria. As vias fora do fato vencem inclusive resposta. (`app/chamados/models.py:3158`, `app/chamados/models.py:3172`) | Evento comum de P. (`app/chamados/models.py:3180`) |
| `fechar_por_admin`; `encerrar_fantasma` | Delegam às portas anteriores; não acrescentam trava. (`app/chamados/models.py:3054`, `app/chamados/models.py:3277`) | Herdado do `save`. (`app/chamados/signals.py:237`) |
| `encerrar_por_fato_no_espelho` | Troca a via de pergunta já materializada; não prova nova morte C6. Sem trava própria. (`app/chamados/models.py:3258`) | `save` da via acorda outbox. (`app/chamados/models.py:3266`; `app/chamados/models.py:3210`) |
| `_carimbar_validacao` | `validada_em`, autor e `DecisaoPergunta`, juntos em atomic; trava herdada. Validação mata no primeiro degrau C6. (`app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:136`; `app/chamados/juizes.py:1153`) | Evento comum de P e sequência de decisão. (`app/chamados/services/validacao.py:137`) |
| `fio.reapontar_resolvedora` | Destino e, condicionalmente, data; atomic sem savepoint; C, destino pela guarda de costura e D/Ch/P adicionais em colisão. Pode retirar a pergunta do chamado antigo sem alterar o status dele. (`app/chamados/services/fio.py:873`, `app/chamados/services/fio.py:881`, `app/chamados/services/fio.py:896`, `app/chamados/services/fio.py:914`) | Outbox vê o estado posterior; não há emissão explícita para a âncora antiga nesse corpo. (`app/chamados/services/fio.py:916`; `app/chamados/models.py:3229`) |
| `PerguntaDisputa._nascer`, reuso | Pode substituir célula/destino de linha existente; atomic e C antes do dedup. (`app/chamados/models.py:2839`, `app/chamados/models.py:2843`, `app/chamados/models.py:2957`) | `ex.save`, logo evento comum de P. (`app/chamados/models.py:2999`) |
| `fio.rechavear_data_evento` | Dia/célula/lavra; atomic e C→D→P. (`app/chamados/services/fio.py:989`, `app/chamados/services/fio.py:993`) | Além do `save`, acorda explicitamente a célula antiga. (`app/chamados/services/fio.py:1026`) |
| `retratar_para_canonica → retratar_duplicata` | Atomic; C/D/P das perdedoras e canônica ordenados; carimbo `duplicata`. (`app/chamados/services/fio.py:1319`, `app/chamados/services/fio.py:1321`, `app/chamados/services/fio.py:1342`) | Evento comum de P. (`app/chamados/models.py:3180`) |
| `DisputaSupervisao.remover_motivo` | Atomic C/D/Ch/P; decisão de retratação e `motivo_retratado`. Com motivos restantes, retorna sem fechar disputa/pai. (`app/chamados/models.py:2023`, `app/chamados/models.py:2026`, `app/chamados/models.py:2054`, `app/chamados/models.py:2064`) | Evento comum de P; a disputa também é salva. (`app/chamados/models.py:2059`, `app/chamados/models.py:2063`) |
| `relavra_via.relavrar` | Troca via/lavra, sem atomic ou trava próprios além do `save`; pode mudar insumo C6. (`app/chamados/services/relavra_via.py:106`, `app/chamados/services/relavra_via.py:125`) | Outbox e auditoria da relavra. (`app/chamados/services/relavra_via.py:127`, `app/chamados/services/relavra_via.py:132`) |
| `responder_pergunta_disputa_core` | Resposta/carimbo, desfaz validação anterior; atomic, C e `exigir_colaboradores_travados`. A concordância resposta/luz pode fazer C6 morrer. (`app/chamados/services/disputa_emissao.py:1368`, `app/chamados/services/disputa_emissao.py:1404`; `app/chamados/juizes.py:1181`) | Outbox e decisão de resposta. (`app/chamados/services/disputa_emissao.py:1409`) |
| `responder_automaticamente`; `carimbar_resposta_pela_trilha` | Resposta/carimbo em atomic, sem trava própria de C; efeito C6 depende da pergunta. (`app/chamados/services/disputa_emissao.py:1539`, `app/chamados/services/disputa_emissao.py:1555`, `app/chamados/services/disputa_emissao.py:1562`, `app/chamados/services/disputa_emissao.py:1576`) | Outbox e sequência de decisão; trilha também registra auditoria. (`app/chamados/services/disputa_emissao.py:1557`, `app/chamados/services/disputa_emissao.py:1578`) |

### Chamadores semânticos e casos em que o chamado pode sobreviver

| Função | Atomicidade/trava e efeito |
|---|---|
| `DisputaSupervisao.fechar` | Atomic C/D/Ch antes de carimbar mudas; salva disputa depois. S88 tenta fechar o pai pelo gate, em best-effort; fechamento do pai não é garantido pelo carimbo. (`app/chamados/models.py:1903`, `app/chamados/models.py:1908`, `app/chamados/models.py:1919`, `app/chamados/models.py:1934`; `app/chamados/signals.py:41`) |
| `lavrar_sem_turno_aberto` | Só carimba após C6 já dizer morta; sem atomic/trava próprios nesse ramo além do `save`. Pode fechar disputa depois. (`app/chamados/reconciliador.py:397`, `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:411`) |
| `retratar_perguntas_absolvidas_por_celula` | Atomic por pergunta, C e exigência do registro de trava; carimba pergunta já absolvida pelo juiz; tenta fechar disputa depois. (`app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:592`, `app/chamados/reconciliador.py:594`, `app/chamados/reconciliador.py:604`) |
| `encerrar_orfas_pela_lampada` | Atomic por pergunta; `encerrar_fantasma` e auditoria; sem aquisição própria de C nesse bloco. (`app/chamados/reconciliador.py:1146`) |
| `_materializar_ausencia_de_resposta` | Atomic por irmã, C, carimbo e decisão de absorção. Ausência aberta mantém destino vivo; ausência decidida pede `reconciliar`. (`app/chamados/services/materializacao.py:557`, `app/chamados/services/materializacao.py:560`, `app/chamados/services/materializacao.py:578`, `app/chamados/services/materializacao.py:581`) |
| `_materializar_resposta_hora` | Paridade recusada usa identidade; herda atomic de `_materializar_uma`. (`app/chamados/services/materializacao.py:924`, `app/chamados/services/materializacao.py:1220`) |
| `_carimbar_recusa` | Colisão usa `fato_ocupado`; pode anexar ocorrência ao destino sem mudar seu status. Sem trava própria; contexto depende do chamador. (`app/chamados/services/materializacao.py:1093`, `app/chamados/services/materializacao.py:1099`, `app/chamados/services/materializacao.py:1105`) |
| `_materializar_uma` | Atomic de plantio/carimbo; pode tocar destino/clones. Portanto somente seus ramos condicionais entram em “sem tocar o chamado”. (`app/chamados/services/materializacao.py:1115`, `app/chamados/services/materializacao.py:1220`, `app/chamados/services/materializacao.py:1277`) |
| `materializar_pendentes_aprovadas` | Carimbo pelo lastro, sem atomic/trava C próprios além do `save`; acionada pelo signal de disputa. (`app/chamados/services/materializacao.py:1418`, `app/chamados/services/materializacao.py:1431`; `app/chamados/signals.py:147`) |
| `_resolver_perguntas_por_chamado_resolvido` | Wrapper abre/preserva atomic e trava C/D/Ch; pode reabrir pai ou fechar disputa. O segundo laço só carimba perguntas já mortas pelo C6. (`app/chamados/services/disputa_emissao.py:1697`, `app/chamados/services/disputa_emissao.py:1764`, `app/chamados/services/disputa_emissao.py:1789`, `app/chamados/services/disputa_emissao.py:1825`) |
| `validar_pergunta` | Atomic C/D/Ch; valida e tenta materialização; pode devolver a validação antes do commit. Morte durável depende do desfecho. (`app/chamados/services/validacao.py:425`, `app/chamados/services/validacao.py:431`, `app/chamados/services/validacao.py:435`, `app/chamados/services/validacao.py:464`) |
| `_aceitar_a_folga` | Os dois ramos têm atomic e C/D/Ch; validam/encerram pergunta e pedem fechamento do fio. (`app/chamados/services/validacao.py:281`, `app/chamados/services/validacao.py:285`, `app/chamados/services/validacao.py:314`, `app/chamados/services/validacao.py:329`) |
| `fechar_o_fio_do_dia` | C/D/Ch; fecha perguntas e depois tenta reconciliar chamados. A recusa do gate pode preservar o chamado. (`app/ponto/services/veredito_celula.py:175`, `app/ponto/services/veredito_celula.py:184`, `app/ponto/services/veredito_celula.py:201`) |
| `encerrar_perguntas_com_fato.handle` | Chama a porta que troca a via de pergunta já materializada. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`; `app/chamados/models.py:3258`) |

Os gatilhos de `remover_motivo` encontrados são reativação de posto e recalibração da âncora. (`app/colaboradores/signals.py:77`; `app/escala/signals.py:161`)

Os chamadores de reapontamento encontrados são `_gate_pergunta_viva`, reemissão, dois ramos de reata órfã, declaração, migração, reata/agrupamento no limbo e reversão administrativa; todos passam pela porta de costura descrita acima. (`app/chamados/models.py:437`; `app/chamados/reconciliador.py:954`, `app/chamados/reconciliador.py:1638`, `app/chamados/reconciliador.py:1651`; `app/chamados/services/declaracao_texto.py:342`; `app/chamados/services/fio.py:2129`; `app/chamados/services/limbo_folgas.py:244`, `app/chamados/services/limbo_folgas.py:282`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:127`)

Rechaveamento também é chamado no reuso e na migração/reversão de `data_evento`. (`app/chamados/services/fio.py:1057`, `app/chamados/services/fio.py:1061`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:400`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:492`)

### Morte por leitura, sem salvar PerguntaDisputa

Esses caminhos não disparam signal de P por si; mudam entradas que C6 lê. (`app/chamados/juizes.py:1155`, `app/chamados/juizes.py:1166`, `app/chamados/juizes.py:1196`, `app/chamados/juizes.py:1216`)

| Escritor da entrada | Transação/trava/evento |
|---|---|
| `celula.lavrar_veredito → salvar_e_emitir` | Veredito/via/ata; atomic de célula+outbox, sem trava própria de C. Com `salvar=False`, depende do salvamento posterior. (`app/ponto/portas/celula.py:969`, `app/ponto/portas/celula.py:1033`, `app/ponto/portas/celula.py:894`) |
| `editar_dia`; regeneração | Edição usa atomic e trava da célula, seguida de rejulgamento; regeneração tem atomic. (`app/ponto/portas/celula.py:592`, `app/ponto/portas/celula.py:627`, `app/ponto/portas/celula.py:218`) |
| `reverter_regeneracao_dia` | Grava DNA/trilha; o corpo termina sem o rabo de rejulgamento da edição. (`app/ponto/portas/celula.py:785`, `app/ponto/portas/celula.py:808`) |
| `trancar_periodo` | Grava `PeriodoFechado`, sem atomic/trava próprios nessa função, e tenta fechamento no ato. A pergunta morre por leitura da tranca; não recebe novo carimbo dessa via. (`app/ponto/services/fechamento.py:1408`, `app/ponto/services/fechamento.py:1419`, `app/ponto/services/fechamento.py:1426`; `app/chamados/services/competencia_trancada.py:177`) |
| `registrar_batida`, `anotar`, `retratar_batida` | Sem atomic/trava C próprios nessas portas; saves despertam cartório/reconciliador pelos signals de Batida. (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:254`, `app/ponto/registro_batida.py:274`; `app/ponto/signals.py:131`, `app/ponto/signals.py:206`) |
| Alterações de Ausência/Justificativa | Seus signals também julgam o dia, podendo mudar a leitura C6 sem salvar P. (`app/ponto/signals.py:143`, `app/ponto/signals.py:161`; `app/chamados/juizes.py:1200`) |

O instante também participa do fallback da janela viva; incidência produtiva de mudança de vitalidade apenas pela passagem do tempo: **[nao verificado]**. (`app/chamados/juizes.py:1217`)

### (2b) Nascimento/reabertura de chamado sem garantia de pergunta viva no mesmo ato

| Porta | Transação/trava e eventos |
|---|---|
| **A — `ChamadoColaborador.abrir`** | Atomic e C antes do dedup. Pode nascer ou renascer. Emite `EventoCelula` para módulo de turno com célula; não garante pergunta viva. (`app/chamados/models.py:902`, `app/chamados/models.py:905`, `app/chamados/models.py:922`, `app/chamados/models.py:971`, `app/chamados/models.py:977`) |
| **C — `criar`** | Atomic sem savepoint para INSERT+evento condicional; não adquire C por conta própria. (`app/chamados/models.py:1093`) |
| **R — `reabrir`** | `_gravar_estado` usa atomic C/D/Ch; depois tenta ressuscitar perguntas em outro atomic. O conjunto pode estar vazio; não cria pergunta nova. (`app/chamados/models.py:280`, `app/chamados/models.py:1432`, `app/chamados/models.py:1441`, `app/chamados/models.py:1533`, `app/chamados/models.py:1547`) |
| **T — transição humana** | O motor permite `resolvido → em_analise`; `_aplicar` só delega ao núcleo de estado, sem ressuscitação explícita. (`app/chamados/catalogo/motor.py:102`; `app/chamados/models.py:447`) |

Todas essas entradas salvam Chamado e podem acionar o cartório; isso é condicionado por nascimento/mudança de status, `raw` e `em_julgamento`. (`app/chamados/signals.py:187`)

**Entradas de furo/turno:**

| Função/sítio | Porta e tentativa de pergunta |
|---|---|
| `detectar_ausencias._abrir_chamado` | A; tentativa posterior de fio em best-effort, cuja falha é engolida. (`app/ponto/management/commands/detectar_ausencias.py:693`, `app/ponto/management/commands/detectar_ausencias.py:790`, `app/

[truncado em 20000 de 44226 caracteres]

## 09/11 01:04 · sessao 604cd303beb9 · - · - · origem=Ronald (tmux)

Consolidado do item 6 — referências abaixo usam o prefixo `app/`; é mapa de funções e usos sem repetir cada ocorrência textual. Os escritores persistentes identificados ficam em `chamados/models.py:971`, `chamados/models.py:1095`, `chamados/models.py:296`, `chamados/models.py:3009`, `chamados/models.py:3019` e `chamados/services/fio.py:924`.

**`PerguntaDisputa.chamado_destino`**

- Campo significa “Chamado órfão que esta pergunta resolve quando materializada”; a lista `chamados_relacionados_ids` representa clones resolvidos junto. `chamados/models.py:3385-3391`, `chamados/models.py:3404-3410`.
- Escritor de nascimento: `_nascer` constrói a pergunta com destino e insere; escritor de reapontamento no reuso: `_nascer` atribui `ex.chamado_destino`, lavra e salva; segunda porta de reapontamento: `fio.reapontar_resolvedora`, atribuição e save com lavra. `chamados/models.py:2985-3009`, `chamados/models.py:3016-3019`, `chamados/services/fio.py:922-925`.
- `_costura_nao_mata_o_fio` atribui temporariamente o destino para julgar e o restaura; `_vivas_entre` faz operação semelhante em memória. Essas atribuições não são novos escritores persistentes. `chamados/services/fio.py:778-783`, `chamados/models.py:356-371`.
- Leitores de trava/encadeamento: `_trava_do_repasse`, `_trava_do_ato`, `DisputaSupervisao.fechar`, `remover_motivo`, validação e fechamento do dia usam o destino para compor conjuntos. `chamados/services/disputa_emissao.py:956-959`, `chamados/services/declaracao_texto.py:295-297`, `chamados/models.py:1919`, `chamados/models.py:2035`, `chamados/services/validacao.py:432`, `ponto/services/veredito_celula.py:186`.
- Leitores de resolução/ciclo: `_gravar_estado`, `_ancoradas`, `_gate_pergunta_viva`, `_resolver_perguntas_por_chamado_resolvido`, `_pergunta_materializada_de_fato`, `irmas_respondidas_sem_consumo`, `via_de_hoje`, materializadores e `becos`. `chamados/models.py:309-312`, `chamados/models.py:345`, `chamados/models.py:436-444`, `chamados/services/disputa_emissao.py:1748`, `chamados/juizes.py:757-758`, `chamados/juizes.py:1770-1773`, `chamados/services/relavra_via.py:101-103`, `chamados/services/materializacao.py:339`, `chamados/services/materializacao.py:527`, `chamados/services/materializacao.py:581-582`, `chamados/services/materializacao.py:711`, `chamados/services/materializacao.py:794`, `chamados/services/materializacao.py:994`, `chamados/services/materializacao.py:1076`, `chamados/services/becos.py:30`, `chamados/services/becos.py:46-47`.
- Leitores de apresentação/identificação: `chamado_que_lista`, `_disputa_do_chamado`, `montar_chamados_fio`, `api_chamados_v2`, `api_disputa_perguntas`, `creditos_perguntas`, histórico, ações de chamado/disputa, janela do colaborador, pergunta no app e explicador. `chamados/services/fio.py:116-137`, `chamados/services/fio.py:225-227`, `api/views.py:1026`, `api/views.py:1059-1065`, `api/views.py:2590`, `api/views.py:2606-2608`, `chamados/services/raiox.py:133-149`, `chamados/services/historico.py:96`, `chamados/services/acoes_chamado.py:357`, `chamados/services/acoes_disputa.py:351`, `chamados/services/janela_colab.py:64-65`, `chamados/services/pergunta_no_app.py:143`, `chamados/services/pergunta_no_app.py:166-169`, `chamados/services/explicador.py:122`.
- Leitores de diagnóstico/manutenção: `vigia_carimbo.classificar`, `competencia_trancada.medir`, `limbo_folgas`, `lavrar_perguntas_stale`, `auditar_disputas`, `auditar_invariantes_chamados`, `reconciliar_fantasmas`, `reconciliar_grade`, commands de relavra/backfill/migração/reabertura. `chamados/services/vigia_carimbo.py:103`, `chamados/services/competencia_trancada.py:108-140`, `chamados/services/limbo_folgas.py:204`, `chamados/services/limbo_folgas.py:239-240`, `chamados/management/commands/lavrar_perguntas_stale.py:57`, `chamados/management/commands/auditar_disputas.py:151`, `chamados/management/commands/auditar_invariantes_chamados.py:588-595`, `ponto/management/commands/reconciliar_fantasmas.py:210-215`, `ponto/management/commands/reconciliar_grade.py:460`, `chamados/management/commands/relavrar_via_i6.py:56`, `chamados/management/commands/backfill_veredito_lavrado.py:101`, `chamados/management/commands/migrar_data_evento_dia_do_turno.py:377`, `chamados/management/commands/reabrir_orfas_enterradas.py:69`.

**Divergência semântica comprovada:** `chamado_que_lista` devolve destino sempre que houver; API e crédito do raio-X usam destino somente se vivo, caindo no pai quando morto; `irmas_respondidas_sem_consumo` segue a regra do crédito. Portanto, “destino”, “âncora de listagem” e “chamado creditado” não são universalmente intercambiáveis. `chamados/services/fio.py:116-117`, `api/views.py:1062-1065`, `chamados/services/raiox.py:140-147`, `chamados/juizes.py:1770-1773`.

**Queries por destino sem filtro de dono:** `_disputa_do_chamado`, histórico, resolução S143, busca/reuso na porta órfã, migração de resolvedoras e relavra do estado. A ausência de filtro é verificável; existência de dados cruzados em produção não veio no anexo. `chamados/services/fio.py:134-136`, `chamados/services/historico.py:96`, `chamados/services/disputa_emissao.py:1748`, `chamados/services/disputa_emissao.py:1002`, `chamados/services/disputa_emissao.py:1022`, `chamados/services/fio.py:2037`, `chamados/models.py:309-311`; dados cruzados em produção: [não verificado].

**`PerguntaDisputa.colaborador`**

- Escritor runtime identificado: `_chave_de_identidade` preenche com `self.disputa.colaborador_id`, ou recusa valor diferente; só é chamado por `save` quando `_state.adding`. Logo, há guarda de nascimento, sem prova de igualdade para qualquer UPDATE/acervo legado. `chamados/models.py:3420-3423`, `chamados/models.py:3430-3435`.
- Escritor histórico: migration 0051 atualiza a coluna por subquery da disputa. `chamados/migrations/0051_decisao_pergunta.py:25`.
- Leitores diretos de identidade/dedup/trava: `q_dos_fatos`, `fato_livre_para`, `fio.chave_do_dia`, `_colab_da_linha`, `reapontar_resolvedora`, `rechavear_data_evento`, `retratar_para_canonica`, `grupos_de_colisao`, migração de resolvedoras e conjunto de repasse. `chamados/models.py:2762-2763`, `chamados/models.py:2774-2781`, `chamados/services/fio.py:1450-1451`, `chamados/services/fio.py:1646-1653`, `chamados/services/fio.py:863`, `chamados/services/fio.py:888-889`, `chamados/services/fio.py:1001`, `chamados/services/fio.py:1022-1025`, `chamados/services/fio.py:1329-1330`, `chamados/services/fio.py:1946`, `chamados/services/fio.py:2085-2092`, `chamados/services/disputa_emissao.py:956`.
- Leitores de resolução/censo: `carimbar_pergunta_c1` usa `p.colaborador` como fallback; contador de destino cruzado compara a coluna ao dono do chamado; backfill de decisões usa a coluna para competência. `chamados/reconciliador.py:368`, `chamados/services/fio.py:1186-1193`, `chamados/management/commands/backfill_decisoes_pergunta.py:69`, `chamados/management/commands/backfill_decisoes_pergunta.py:114-120`.
- Outros leitores continuam derivando o dono pela disputa: `qs_perguntas_absolviveis`, `_orfas_abertas`, `perguntas_orfas_reatavies`, juiz de tranca, validação, autorização da resposta no fio e staleness da lavra. Se coluna e disputa divergirem, esses universos não são necessariamente iguais aos anteriores. `chamados/reconciliador.py:627-628`, `chamados/reconciliador.py:1052-1053`, `chamados/reconciliador.py:1496-1501`, `chamados/juizes.py:949`, `chamados/services/validacao.py:268`, `chamados/services/validacao.py:431`, `chamados/services/fio.py:598`, `chamados/veredito_lavrado.py:210-213`.
- A costura atual recusa destino de outro dono tanto no nascimento quanto no reapontamento; essa recusa não é validação global das relações já gravadas. `chamados/models.py:2848`, `chamados/services/fio.py:863-865`, `chamados/services/fio.py:1575-1579`.

**`ChamadoColaborador.status_local`**

- Escritores runtime persistentes: nascimento `abrir` define `_def['status_local']` e passa ao `get_or_create`; `criar` passa estado ao `create`; transições atribuem em `_gravar_estado` e salvam. `chamados/models.py:938-940`, `chamados/models.py:971`, `chamados/models.py:1095`, `chamados/models.py:228`, `chamados/models.py:296`.
- Chamadores diretos do escritor de transição: `_aplicar`, `reconciliar`, `cancelar`, `analisar`, `reabrir`, `encerrar`, `encerrar_por_competencia_trancada`. `chamados/models.py:450`, `chamados/models.py:654`, `chamados/models.py:788`, `chamados/models.py:806`, `chamados/models.py:1442`, `chamados/models.py:1616`, `chamados/models.py:1636`.
- Principais leitores do ato: conjuntos de trava; dedup/renascimento; guards de fechamento/reabertura; receivers de Chamado; reconciliador; cartório; C6; materializador; fio; crédito/API. `chamados/models.py:2691`, `chamados/models.py:921`, `chamados/models.py:927`, `chamados/models.py:673`, `chamados/models.py:1337-1358`, `chamados/signals.py:33`, `chamados/signals.py:70`, `chamados/signals.py:86-96`, `chamados/signals.py:184-192`, `chamados/signals.py:272-280`, `chamados/signals.py:341-345`, `chamados/reconciliador.py:839`, `chamados/reconciliador.py:936-938`, `chamados/reconciliador.py:997`, `chamados/reconciliador.py:1204`, `chamados/reconciliador.py:1450`, `chamados/reconciliador.py:1583`, `ponto/services/cartorio.py:1334`, `ponto/services/cartorio.py:1592`, `ponto/services/cartorio.py:1611`, `chamados/juizes.py:758`, `chamados/services/materializacao.py:773-779`, `chamados/services/fio.py:770`, `api/views.py:1063`.
- Leitores de fila/tela/diagnóstico incluem painel, fila por verbo, `na_fila`, `pergunta_no_app`, `vigia_fio`, calendário/ficha/drawer, relatórios, trabalho do DP, regeneração, cobertura, triagem, ausência, competência trancada, SLA e commands emissores/reconciliadores. Exemplos concretos: `chamados/services/painel.py:155`, `chamados/services/painel.py:187`, `chamados/services/verbos.py:230`, `chamados/services/na_fila.py:17`, `chamados/services/pergunta_no_app.py:129`, `chamados/services/vigia_fio.py:607-618`, `colaboradores/services/calendario.py:437-444`, `colaboradores/services/ficha.py:163`, `colaboradores/services/drawer.py:89`, `relatorios/views.py:745`, `ponto/worklist.py:40`, `escala/services/regeneracao.py:170`, `ponto/services/cobertura.py:226`, `ponto/services/triagem_batida.py:290`, `ponto/services/ausencia.py:709`, `chamados/services/competencia_trancada.py:88`, `chamados/services/sla.py:37`, `chamados/management/commands/reconciliar_chamados.py:59`, `ponto/management/commands/detectar_ausencias.py:739`.

A coluna representa estado do ciclo, mas os leitores fazem perguntas diferentes: `VIVOS` inclui `registrado`, enquanto `COBRANCA` o exclui; `ENCERRADOS` exclui `cancelado/superado`, que pertencem a `TERMINAIS`; portanto “vivo”, “cobra”, “encerrado” e “terminal” não são sinônimos. `chamados/catalogo/motor.py:24-32`. O véu de silêncio também é campo separado do estado. `chamados/models.py:87-98`.

**Leitura do juiz versus escrita de lavra**

`pergunta_viva` calcula e guarda memo apenas na instância; `celula_do_dia_agora` faz SELECT e projeção da ata/veredito em memória; `fato_do_marco_no_espelho` lê a ata. Não encontrei escrita de P/célula nesses corpos. `chamados/juizes.py:850-863`, `chamados/juizes.py:388-410`, `chamados/juizes.py:904-931`. Já `veredito_lavrado.lavrar` salva Pergunta por padrão, salvo `salvar=False`; o backfill persiste por `gravar_lavras_em_lote`. `chamados/veredito_lavrado.py:234-255`, `chamados/veredito_lavrado.py:220-230`.

## 09/11 01:04 · sessao d079295fe0af · - · - · origem=Ronald (tmux)

O censo das cascatas encontrou caminhos fora do teto inicial: terminais sem pergunta prévia, perguntas selecionadas pelo dono da disputa em vez da coluna `PerguntaDisputa.colaborador`, pais alcançados por FK e até outros colaboradores pela limpeza do token FCM. O teto inicial está em `app/chamados/services/disputa_emissao.py:954-959`; os caminhos estão detalhados abaixo.

| Caminho | Escritas/travas alcançadas e cobertura |
|---|---|
| **S88** | Disputa fechada → busca pai somente por `pk`, sem filtro de colaborador → `ch.reconciliar()` → nota. Cadeia: `app/chamados/signals.py:18-45`. Pai estrangeiro pode ser alcançado se a FK estiver assim; existência dessa situação em produção: **[nao verificado]**. |
| **S143/S128** | Chamado encerrado → `resolver_perguntas_por_chamado_resolvido()` em `app/chamados/signals.py:90-115` → trava universo do colaborador do chamado em `app/chamados/services/disputa_emissao.py:1720-1722` → busca perguntas **somente por `chamado_destino`**, sem filtro de colaborador, em `:1747-1751` → carimbo em `:1787-1788`. |
| **Cauda do resolver** | Inclui disputas das perguntas encontradas, inclusive de outro dono, em `app/chamados/services/disputa_emissao.py:1804`, `:1827-1829`; pode reabrir pai terminal em `:1807-1814`, carimbar outras perguntas dessas disputas em `:1836-1861` e fechar disputas em `:1875-1888`. |
| **S144** | Fecho de disputa → `materializar_pendentes_aprovadas()` em `app/chamados/signals.py:147-163` → todas as P pendentes daquela disputa, sem filtro pela coluna colaborador, em `app/chamados/services/materializacao.py:1418-1431` → `PerguntaDisputa.carimbar()` → `save()` em `app/chamados/models.py:3152`. |
| **Cartório síncrono** | Mudança de status/nascimento de Chamado → `julgar_celula(instance.colaborador, dia)` em `app/chamados/signals.py:187-203` → célula do par colaborador/data em `app/ponto/services/cartorio.py:1976-1989`. |
| **E2 do cartório** | `app/ponto/services/cartorio.py:958-963` → perguntas por **`disputa__colaborador`**, inclusive disputa fechada, em `app/chamados/reconciliador.py:627-641`; carimba em `:595`, fecha disputas em `:612-614`. Se a coluna P.colaborador divergir do dono da disputa, a P pode faltar no teto que usa a coluna em `disputa_emissao.py:956`. |
| **Chamados do cartório** | Reconcilia vivos em `app/ponto/services/cartorio.py:1488`, `:1501`, `:1522`, `:1600`, `:1615`; ressuscita perguntas em `:1622`; atualiza marcos/contexto dos vivos em `:1789` e `:1798` → `app/ponto/management/commands/detectar_ausencias.py:558-567` → trava/save em `app/chamados/services/contexto.py:103-104`. |
| **Terminal pelo emissor** | Cartório → emissor em `app/ponto/services/cartorio.py:1740` ou `:1778` → `ChamadoColaborador.abrir(colaborador, modulo, data)` **sem status no lookup**, em `app/ponto/management/commands/detectar_ausencias.py:693-713` → dedup `app/chamados/models.py:906` → terminal `ENCERRADOS` renasce em `:921-923` → `_renascer_por_premissa_viva()` em `:983-1023`. Se terminal não for órfão/destino previamente coletado, não está no teto inicial. |
| **Terminal de cluster** | Cartório → cluster em `app/ponto/services/cartorio.py:990-994` → dias da janela de 14 dias em `app/ponto/services/cluster_espurio.py:12`, `:40-49` → `abrir()` sem filtro de status em `:127-147` → mesmo renascimento acima. Também pode faltar no teto inicial. |
| **Fábrica após lavra** | `app/ponto/services/cartorio.py:985-997` → `app/chamados/services/fabrica_da_celula.py:174-186` → desfaz carimbo: trava C pela **coluna da P**, D e P em `app/chamados/services/relavra_via.py:223-232`, descarimba em `:246`; save em `app/chamados/models.py:3212`. Julgamento passa `reabrir_fechada=False` em `fabrica_da_celula.py:176`. |
| **Reatar órfã** | Deriva vivo em `app/chamados/reconciliador.py:1552-1553`; histórico `ENCERRADOS` do colaborador/dia em `:1582-1585`; histórico único → reaponta em `:1638` e resolver em `:1644`. Sem histórico → carimbo em `:1615` ou reemissão em `:1617` → `abrir()` em `:910-922`, sem filtro de estado; depois reaponta em `:954`. |
| **Lavrar sem turno** | P.carimbar em `app/chamados/reconciliador.py:405`, fechamento da disputa em `:411-413` → S88/S144 acima. |
| **Reconciliador completo** | `app/chamados/reconciliador.py:1428-1437`: E2, reatar órfãs e retratar premissa morta. Este último percorre todos os chamados vivos do colaborador em `:1449-1460`; `_retratar` grava também Justificativa relacionada pendente em `:529-535`, resolve P em `:536` e anexa nota em `:538-540`. |

Auxiliares síncronos, fora das quatro tabelas da autoridade:

- **CelulaDia**: cartório lavra em `app/ponto/services/cartorio.py:1802`, carimba em `:1822`; `app/ponto/portas/celula.py:1067` → save `:900`. Precedência também grava ata: `cartorio.py:1997` → `celula.py:1082`.
- **EventoCelula**: save da célula pode emitir em `app/ponto/portas/celula.py:901-902`, INSERT em `:891`. Save de P também emite: `app/chamados/signals.py:237-244` → `app/chamados/models.py:3239-3248` → mesmo INSERT.
- **FollowupChamado**: INSERT em `app/chamados/services/nota.py:103` ou `:107`; contexto associado pode gravar Chamado em `:110-123` → `app/chamados/services/contexto.py:162-163`.
- **LogAuditoria**: chamadas `evento/evento_kw` convergem ao INSERT em `app/core/observ.py:134-137`.
- **User sistema**: pode nascer por `get_or_create`, apesar da docstring dizer que já existe: `app/core/utils.py:18`.
- **Pauta/ContadorPauta**: cartório → fábrica, limbo exportado, em `app/chamados/services/fabrica_da_celula.py:181-182`, `:245-251` → INSERT Pauta em `app/pautas/services.py:180`, contador em `:188` → `get_or_create` em `:352`, UPDATE em `:354`; auditoria em `:371`.
- **PushSubscription**: push pode apagar subscription expirada em `app/chamados/push_utils.py:49-51`.
- **Colaborador de outro dono pelo FCM**: `app/chamados/services/disputa_emissao.py:1221-1222`, `:1699` → `app/chamados/fcm_utils.py:101-106`, `:89-94` → `_limpar_token()` consulta **todos com mesmo token**, sem pk/dono, em `:41` → `app/colaboradores/fcm.py:60-62` → UPDATE em `app/colaboradores/services/colaborador.py:88`. Ocorrência de token compartilhado em produção: **[nao verificado]**.
- **Limbo do emissor**: `app/ponto/management/commands/detectar_ausencias.py:760-761` → `app/chamados/services/limbo_folgas.py:274` trava C/D abertas/Ch furo+pai; contexto em `:275`, reaponta perguntas em `:281-282`, reconcilia em `:283`, nota em `:292`.

Callbacks `on_commit` precisam ser separados do atomic original: sua declaração está nos pontos abaixo; a trava original não constitui proteção dessas transações posteriores. `app/chamados/signals.py:144`, `:225`, `:293`, `:356`.

- **Becos**: save de P agenda em `app/chamados/signals.py:214-225` → `app/chamados/services/becos.py:72-78` → trava universo D/Ch em `:51-52`, resolver de cada destino terminal em `:53-54`, fechar D em `:56-57`.
- **Silenciar isento**: nascimento/retorno vivo agenda em `app/chamados/signals.py:269-293` → todos os chamados vivos do colaborador isento em `app/chamados/services/silencio_isentos.py:24-31`, `:55-58` → trava/save do chamado em `app/chamados/services/arquivo.py:170-173`.
- **Filho morto**: agenda reconciliador completo em `app/chamados/signals.py:337-356`.
- **Retomar regularizações**: fecho D agenda em `app/chamados/signals.py:129-144` → reg confirmada do colaborador em `app/chamados/services/regularizacao_ext.py:218-224`. Grava Item em `:139`, `:163`, `:188`; RegularizacaoExterna em `:200`; cria Batida via `:77` → `app/ponto/registro_batida.py:157`; corrige Batida em `regularizacao_ext.py:155`, apaga em `:136`, encerra `reg.chamado` em `:205`. **Batida alvo é só `item.batida_alvo`, sem conferir dono**, em `:127`; FK inconsistente pode levar às cascatas de outro colaborador. Ocorrência em produção: **[nao verificado]**.
- **Cascatas da Batida retomada**: post-save/post-delete recomputam em `app/ponto/signals.py:35-36`, `:49-50` → `:26-28` → DELETE TurnoMaterializado por recorte em `app/ponto/nucleo.py:80-82`, bulk INSERT em `:59`, EventoCelula em `:66`, `:93`. Cartório em `signals.py:135`, `:140`; reconciliador em `:215`; fecho adesão em `:234` → `app/colaboradores/services/pos_vinculo.py:36-45`.
- **Fechamento depois da Batida**: `app/ponto/registro_batida.py:160`, `:175-178` → novo `on_commit` em `app/ponto/services/fechamento.py:1824` → recalcular só colaborador em `:1810` → FechamentoMensal `get_or_create` em `:898`, save em `:1144` ou UPDATE em `:1160`; DiaPago em `:1191-1201` → DELETE e bulk INSERT em `app/ponto/services/dia_pago.py:359-360`. Outros efeitos internos da derivação financeira não foram abertos exaustivamente: **[nao verificado]**.

## 09/11 01:03 · sessao abd3cf758dcc · - · - · origem=Ronald (tmux)

Item 7 — testes que conferem a ordem do agrupador: [nao verificado]

- `app/chamados/tests/test_e1_fila.py::F1AgrupadorTest::test_RED_o_repasse_trava_C_D_Ch_P_no_topo`: executa o agrupador com duas cobranças órfãs e pergunta reutilizável em disputa fechada; exige que o repasse avance, que a disputa apareça na sequência de travas e que `violacoes_linhas(seq)` seja vazio. `app/chamados/tests/test_e1_fila.py:240-278`.
- `app/chamados/tests/test_e1_fila.py::F1AgrupadorTest::test_MORDE_sem_a_trava_do_repasse`: substitui `_trava_do_repasse` pela trava apenas do colaborador e exige uma violação na sequência. `app/chamados/tests/test_e1_fila.py:280-286`.
- `app/chamados/tests/test_e1_fila.py::F1AgrupadorTest::test_RED_o_teto_do_repasse_cobre_TODA_pergunta_do_colaborador`: chama somente a helper inicial; exige a presença de uma pergunta de vinte dias antes e da disputa dela nas linhas travadas. Não executa o agrupador nesse trecho. `app/chamados/tests/test_e1_fila.py:288-301`.
- `app/chamados/tests/test_e1_volta2.py::OrdemDeTravaTest::test_MORDE_agrupador_trava_o_colaborador_antes_de_gravar_a_disputa`: inspeção AST; exige `_trava_do_repasse` como primeira instrução do primeiro `with` e primeira chamada dessa helper igual a `PerguntaDisputa.trava_na_ordem(colaborador.pk)`, sem argumentos adicionais. `app/chamados/tests/test_e1_volta2.py:176-194`.

O detector SQL registra a primeira aquisição de cada `(tabela, pk)` por `UPDATE` ou `FOR UPDATE`; cobra ordem de classe, pk crescente e ausência de linha não sondada. Não compara escritas com o conjunto calculado no topo. Portanto uma pergunta preexistente fora desse conjunto, escrita posteriormente com pk maior, pode passar; uma nova trava de C/D/Ch depois de P seria acusada se o cenário do teste alcançasse esse caminho. `app/chamados/tests/test_e1_v9.py:46-116`.

`executemany`, travas de FK dos `INSERT`s e os `INSERT`s não entram nessa captura; o executor do teste também exclui linhas com pk acima do máximo anterior ao ato. `app/chamados/tests/test_e1_v9.py:51-53`, `app/chamados/tests/test_e1_fila.py:59-71`.

A suíte mais antiga `test_e1_v5_ordem_trava.py` registra apenas a primeira aquisição por **tabela**, não por linha, e declara que não vê a segunda disputa adquirida depois de P; não serve como prova de cobertura do conjunto inicial. `app/chamados/tests/test_e1_v5_ordem_trava.py:35-49`, `app/chamados/tests/test_e1_v5_ordem_trava.py:362-367`.

Item 5 — conjuntos das outras portas abertas:

| Porta | Conjunto inicial e continuação |
|---|---|
| `declarar_hora_colab` / `aplicar_declaracao` | Compartilham `_trava_do_ato`: C; disputas abertas/vivos de C; perguntas do universo **do fato específico**, suas disputas/destinos; chamado da declaração. `app/chamados/services/declaracao_texto.py:282-297`. `aplicar_declaracao` chama a helper antes da emissão, abre/reutiliza P e responde; a validação acontece **fora** desse atomic. `app/chamados/services/declaracao_texto.py:648-690`. A validação abre outra transação com disputa/pai/destino/clones e universo abertas/vivos, antes de carimbar/materializar. `app/chamados/services/validacao.py:425-435`, `app/chamados/services/validacao.py:452-459`. |
| `reabrir_orfas_enterradas::Command.handle` | C da disputa; D própria; pai; abertas/vivos; somente P própria. Depois descarimba P, reabre D e chama `pai.reabrir()`. `app/chamados/management/commands/reabrir_orfas_enterradas.py:146-172`. A reabertura do pai escreve status e ressuscita perguntas, portanto volta ao alcance de cartório/S143/ressuscitação analisado pelo agente principal. `app/chamados/models.py:1442-1451`, `app/chamados/models.py:1550-1572`. |
| `fio.migrar_resolvedoras_vivas` | `_conjunto_da_migracao` busca todas as P costuradas no velho ou novo, **sem filtro de colaborador**, e acrescenta o universo de cada linha. `app/chamados/services/fio.py:2032-2039`. Trava donos dessas P + donos de velho/novo, D das P, os dois Ch, P; relê e recusa crescimento de C/P/D. Não inclui automaticamente todos os pais, vivos ou abertas. `app/chamados/services/fio.py:2084-2099`. |
| `migrar_data_evento_dia_do_turno::Command._migrar` e `_reverter` | Travam C/D/destinos/P somente das linhas dos grupos ou arquivo de reversão; depois chamam `rechavear_data_evento`. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:374-400`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:465-492`. O re-chaveamento escreve a P e emite evento também para a célula antiga. `app/chamados/services/fio.py:1033-1039`. |
| `fio._costura_nao_mata_o_fio` | Dentro de atomic, trava C do destino e o destino; relê seu status. `app/chamados/services/fio.py:761-770`. |
| `fio.reapontar_resolvedora` | Trava os C da linha/destino; somente no ramo com colisão acrescenta D/P da linha e perdedoras e Ch destino; depois costura e salva P. `app/chamados/services/fio.py:881-925`. |
| `fio.rechavear_data_evento` | Trava C/D/P, sem Ch; salva P e acorda a célula antiga. `app/chamados/services/fio.py:997-1003`, `app/chamados/services/fio.py:1033-1039`. |
| `fio.retratar_para_canonica` | Trava C/D/P da canônica e perdedoras; chama a porta de carimbo de identidade para cada perdedora. `app/chamados/services/fio.py:1327-1332`, `app/chamados/services/fio.py:1348-1353`. |

A classe de risco deve ser demonstrada pelo alcance efetivo da continuação, e não apenas pela ausência de uma relação nesse quadro: `post_save(Pergunta)` agenda o beco por `on_commit`, enquanto o receiver síncrono de outbox apenas chama `acordar_celulas`; logo callbacks pós-commit não estão protegidos pelas travas da transação original. `app/chamados/signals.py:214-225`, `app/chamados/signals.py:237-244`, `app/chamados/models.py:3231-3248`.

Cuidado para a consolidação: `DisputaSupervisao.reabrir_sistema` anexa nota ao pai, mas essa chamada **não prova UPDATE do Chamado**: sem `contexto_novo`, `anexar_ocorrencia` insere apenas `FollowupChamado`. `app/chamados/models.py:2185-2188`, `app/chamados/services/nota.py:102-111`. Além disso, o reuso de `_nascer` salva a P e retorna; esse trecho não reabre a disputa fechada. `app/chamados/models.py:3000-3010`.

## 09/11 00:58 · sessao a8f8d399adf2 · - · - · origem=Ronald (tmux)

Censo da pergunta (2), somente código. A distinção necessária é entre escrever `materializada_em` e matar uma pergunta pelo C6: `carimbar` só grava se o C6 a der por morta COM o carimbo; há chamadores que só chegam quando ela JÁ está morta por leitura. (`app/chamados/models.py:3128`; `app/chamados/reconciliador.py:397`; `app/chamados/services/disputa_emissao.py:1825`)

**Portas de escrita da pergunta e encaixe de evento**

| Porta/função | Escrita, atomicidade e trava |
|---|---|
| `PerguntaDisputa.carimbar` | Escreve carimbo/via, opcionalmente resposta/validação/lavra, sem escrever Chamado; pergunta ao C6 antes de salvar. Não toma trava C própria. `app/chamados/models.py:3093`, `:3118`, `:3128`, `:3142`. |
| `carimbar_identidade` | Escreve carimbo/via/recusa sem consultar C6; sem trava C própria. `app/chamados/models.py:3158`, `:3172`, `:3180`. |
| `fechar_por_admin` | Delega à identidade. `app/chamados/models.py:3054`. |
| `encerrar_fantasma` | Delega a `carimbar`. `app/chamados/models.py:3269`, `:3277`. |
| `encerrar_por_fato_no_espelho` | Só muda via de pergunta já materializada, não necessariamente transição C6 viva→morta; `save`, sem trava própria. `app/chamados/models.py:3258`, `:3265`. |
| `_carimbar_validacao` | `validada_em` mata no degrau 0; atomic próprio e sequência DecisaoPergunta, trava herdada. `app/chamados/services/validacao.py:127`, `:134`, `:136`; `app/chamados/juizes.py:1153`. |
| `fio.reapontar_resolvedora` | Muda destino e eventualmente data; atomic `savepoint=False`; trava C, e D/CH/P quando a colisão retrata outras linhas. Salva só Pergunta/lavra. `app/chamados/services/fio.py:873`, `:881`, `:896`, `:914`. |
| `_nascer`, ramo de reuso | Pode atualizar célula/destino de pergunta existente; atomic e C no topo; salva Pergunta/lavra. `app/chamados/models.py:2839`, `:2843`, `:2957`, `:2973`, `:2999`. |
| `fio.rechavear_data_evento` | Muda data/célula/lavra sem escrever Chamado; atomic, C→D→P; emite para célula velha além do signal da nova. `app/chamados/services/fio.py:989`, `:993`, `:1020`, `:1026`, `:1028`. |
| `retratar_para_canonica`→`retratar_duplicata` | Atomic; C/D/P ordenados para canônica e perdedoras; via duplicata por identidade. `app/chamados/services/fio.py:1319`, `:1321`, `:1324`, `:1342`. |
| `DisputaSupervisao.remover_motivo` | Atomic C/D/CH/P, DecisaoPergunta.RETRATACAO e via motivo_retratado; com motivos restantes não fecha disputa/pai. `app/chamados/models.py:2023`, `:2026`, `:2054`, `:2059`, `:2064`. |

Todo `PerguntaDisputa.save` reúne save e signal no mesmo atomic; não exige nem toma genericamente trava C. Os saves relevantes acima acionam outbox síncrono e fechamento de beco `on_commit`; o outbox só acorda a célula alcançada depois da escrita, salvo a emissão explícita da célula velha em `rechavear_data_evento`. (`app/chamados/models.py:3403`; `app/chamados/signals.py:214`; `:237`; `app/chamados/models.py:3229`; `app/chamados/services/fio.py:1027`)

**Chamadores semânticos dos carimbos**

| Função/sítio | Trava/transação e qualificação |
|---|---|
| `DisputaSupervisao.fechar`→`fechar_por_admin` | Atomic, C→D→CH antes das perguntas; fecha disputa depois e o signal tenta fechar pai. Pode deixar Chamado cuja premissa ainda vive. `app/chamados/models.py:1903`, `:1908`, `:1916`, `:1934`; `app/chamados/signals.py:41`. |
| `remover_motivo` | Ver porta acima; com outros motivos o Chamado não é escrito por essa função. `app/chamados/models.py:2059`, `:2064`. |
| `lavrar_sem_turno_aberto` | Carimba somente pergunta que C6 JÁ dá por morta; pode fechar disputa depois. Sem atomic/trava própria nesse ramo, além do save P; não é morte C6 originada pelo carimbo. `app/chamados/reconciliador.py:397`, `:405`, `:411`. |
| `retratar_perguntas_absolvidas_por_celula` | Atomic por pergunta, trava C e exige registro da trava, `carimbar(celula_absolveu)`; tenta fechar disputas depois. `app/chamados/reconciliador.py:584`, `:592`, `:594`, `:595`, `:604`. |
| `encerrar_orfas_pela_lampada` | Atomic por pergunta e evento de auditoria; sem trava C própria nesse bloco; `encerrar_fantasma`. `app/chamados/reconciliador.py:1146`, `:1147`, `:1149`. |
| `_materializar_ausencia_de_resposta` | Absorve irmãs em atomic por irmã, trava C, carimbo e DecisaoPergunta.ABSORCAO; ausência ABERTA mantém destino vivo, ausência decidida pede reconciliação. `app/chamados/services/materializacao.py:557`, `:560`, `:561`, `:563`, `:578`, `:581`. |
| `_materializar_resposta_hora` | Recusa por paridade carimba identidade; herda atomic de `_materializar_uma`. `app/chamados/services/materializacao.py:791`, `:924`, `:1220`. |
| `_carimbar_recusa` | Colisão grava `fato_ocupado`, depois pode anexar ocorrência ao destino; não muda seu status. Sem atomic/trava própria fora do save P; depende do caminho chamador. `app/chamados/services/materializacao.py:1093`, `:1099`, `:1105`. |
| `_materializar_uma` | Atomic plantio/carimbo; pode tocar destino/clones pelos ramos de materialização, portanto não deve ser anunciado indiscriminadamente como “sem tocar Chamado”. `app/chamados/services/materializacao.py:1115`, `:1220`, `:1263`, `:1277`. |
| `materializar_pendentes_aprovadas` | Carimba aprovadas pelo juiz do lastro; não toma atomic/trava C próprios além do save P; caller signal da disputa. `app/chamados/services/materializacao.py:1418`, `:1427`, `:1431`; `app/chamados/signals.py:147`. |
| `_resolver_perguntas_por_chamado_resolvido` | Wrapper mantém atomic existente ou abre um, trava C/D/CH; carimba em dois laços; pode reabrir pai ou fechar disputa. Segundo laço só toca pergunta que C6 já dá por morta. `app/chamados/services/disputa_emissao.py:1697`, `:1698`, `:1764`, `:1789`, `:1825`, `:1838`. |
| `validar_pergunta` | Atomic com C/D/CH antes do carimbo; pode materializar, devolver validação e reconciliar. Morte durável depende do desfecho. `app/chamados/services/validacao.py:425`, `:431`, `:435`, `:461`, `:464`. |
| `_aceitar_a_folga` | Ambos os ramos têm atomic e C/D/CH; validam/encerram P e pedem fechamento do fio inteiro. `app/chamados/services/validacao.py:281`, `:285`, `:288`, `:296`, `:314`, `:317`, `:329`. |
| `fechar_o_fio_do_dia` | Trava C→D→CH, fecha perguntas, depois tenta reconciliar chamados; um chamado pode sobreviver à recusa do gate. `app/ponto/services/veredito_celula.py:175`, `:184`, `:189`, `:201`. |
| `encerrar_perguntas_com_fato.handle` | Chama porta da troca de via; não confundir com criar carimbo ou provar morte. `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`; `app/chamados/models.py:3258`. |

Chamadores de reapontamento identificados por AST: `ChamadoColaborador._gate_pergunta_viva` (`app/chamados/models.py:437`); `reemitir_chamado_do_dia` (`app/chamados/reconciliador.py:954`); `reatar_pergunta_orfa`, dois ramos (`app/chamados/reconciliador.py:1638`, `:1651`); `declarar_hora_colab` (`app/chamados/services/declaracao_texto.py:342`); `_migrar_sob_trava` (`app/chamados/services/fio.py:2129`); `reatar_perguntas_ao_limbo` e `agrupar_furo` (`app/chamados/services/limbo_folgas.py:244`, `:282`); reversão administrativa (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:127`). A porta governa travas/eventos; cada efeito C6 concreto depende da pergunta e célula, sem dado produtivo correspondente `[nao verificado]`.

Chamadores de rechaveamento: `rechavear_no_reuso` (`app/chamados/services/fio.py:1057`, `:1061`), migração e reversão (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:400`, `:492`).

**Morte sem escrita na Pergunta**

C6 também muda por leitura de tranca, via admin da célula, concordância resposta/luz, marco no espelho, veredito/ata, `classificar_falta`, janela viva e lastro; esses caminhos não necessitam `save()` na Pergunta nem disparam seu signal por si. (`app/chamados/juizes.py:1155`, `:1166`, `:1181`, `:1196`, `:1200`, `:1214`, `:1216`, `:1234`)

- `lavrar_veredito` escreve veredito/via/ata e usa `salvar_e_emitir`: atomic e EventoCelula, sem trava C própria; `save=False` depende do salvamento posterior do chamador. (`app/ponto/portas/celula.py:969`, `:1027`, `:1033`, `:1034`, `:894`)
- Grade: `editar_dia` tem atomic+trava Celula e rabo comum de rejulgamento; regeneração tem atomic; reversão de regeneração grava DNA/trilha e termina sem chamar esse rabo. (`app/ponto/portas/celula.py:592`, `:593`, `:627`, `:422`, `:218`, `:785`, `:808`)
- `trancar_periodo` grava PeriodoFechado e chama fechamento da fila best-effort, sem atomic/trava C próprios nessa função; perguntas morrem pela LEITURA e não são carimbadas. (`app/ponto/services/fechamento.py:1408`, `:1419`, `:1426`; `app/chamados/services/competencia_trancada.py:177`)
- `registrar_batida`, `anotar`, `retratar_batida` fazem save/create e despertam cartório/reconciliador pelos signals Batida; não contêm atomic/trava C próprios. (`app/ponto/registro_batida.py:152`, `:157`, `:254`, `:274`; `app/ponto/signals.py:131`, `:206`)
- Ausência e Justificativa também fazem julgar o dia pelos respectivos signals. (`app/ponto/signals.py:143`, `:161`)
- O instante sozinho é insumo do ramo da janela viva; efeito produtivo concreto sem sonda `[nao verificado]`. (`app/chamados/juizes.py:1217`)

**Criação/reabertura de Chamado sem garantia de pergunta viva no mesmo ato**

As três portas suficientes para encaixe do evento são:

- `abrir`: atomic+trava C antes do dedup; nasce ou renasce; emissão de EventoCelula somente módulo turno com célula, sem criação de pergunta garantida dentro da porta. (`app/chamados/models.py:902`, `:905`, `:921`, `:971`, `:977`)
- `criar`: atomic `savepoint=False` junta INSERT e evento condicional; não toma C própria. (`app/chamados/models.py:1093`, `:1094`, `:1095`, `:1097`)
- `reabrir`/renascimento: `_gravar_estado` tem atomic+trava C/D/CH, depois `ressuscitar_perguntas` em outro atomic; pode reabrir com zero perguntas ressuscitáveis, não nasce pergunta nova. (`app/chamados/models.py:280`, `:290`, `:1432`, `:1441`, `:1533`, `:1547`)

Reaberturas diretas encontradas:

`_renascer_por_premissa_viva`→`reabrir` (`app/chamados/models.py:1019`), invocado no dedup (`:922`) e em resposta tardia (`app/chamados/services/resposta_tardia.py:45`); `reabrir_orfas_enterradas.handle` (`app/chamados/management/commands/reabrir_orfas_enterradas.py:172`); `sincronizar_adesao` (`app/chamados/services/adesao.py:92`); reabertura da tranca (`app/chamados/services/competencia_trancada.py:265`); resposta aguardando validação (`app/chamados/services/disputa_emissao.py:1789`); desarquivamento (`app/chamados/views_arquivar.py:70`); `detectar_ausencias.handle` (`app/ponto/management/commands/detectar_ausencias.py:301`); `reabrir_chamados_ausencia_aberta.handle` (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`); `reconciliar_grade._emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:530`).

`acao_reabrir_disputa_admin` cria pergunta de CONVERSA antes de reabrir Chamado; não cabe afirmar “nenhuma pergunta criada”, mas ela não garante pergunta de turno. (`app/chamados/services/acoes_disputa.py:260`, `:267`)

Criações de furo/turno com tentativa posterior de fio, sem garantia do mesmo ato:

- `detectar_ausencias._abrir_chamado`: `abrir` em `:693`, tentativa best-effort em `:790`–`:803`. (`app/ponto/management/commands/detectar_ausencias.py:693`, `:790`, `:802`)
- `processar_alertas_turno._abrir_chamado_critico`: `abrir` em `:335`, só tenta fio se `criado`, em bloco que engole falha; renascimento não passa nesse `if`. (`app/ponto/management/commands/processar_alertas_turno.py:335`, `:372`, `:384`, `:385`)
- `processar_alertas_avancados.handle`: `criar` em `:121`, fio best-effort em `:147`–`:160`. (`app/ponto/management/commands/processar_alertas_avancados.py:121`, `:147`, `:158`, `:159`)
- `emitir_turno_aberto_retroativo.handle` e `reconciliar_grade._emitir_furo` só usam nascimento/reabertura, sem invocação de fábrica/perguntas no arquivo. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`; `app/ponto/management/commands/reconciliar_grade.py:507`, `:530`)

Demais entradas `abrir`/`criar` encontradas por AST, cujo pertencimento ao fio mudo exige célula e estado efetivamente gravados `[nao verificado]`:

| Sítio | Referência |
|---|---|
| API justificativa; abertura manual | `app/api/views.py:1997`, `:2201` |
| Auditoria desvio leve API, incluindo cópia | `app/api/views.py:747`; `app/api/views_core.py:747` |
| Abertura local/DP | `app/chamados/services/abertura.py:47`, `:172` |
| Adesão; rápido DP | `app/chamados/services/adesao.py:74`; `app/chamados/views.py:973` |
| Regularização externa | `app/chamados/services/regularizacao_ext.py:246` |
| Desligamento; aceite vínculo | `app/colaboradores/services/desligamento.py:163`; `app/colaboradores/services/vinculo.py:393` |
| Benefício; aceite agenda | `app/colaboradores/views.py:1978`, `:2363` |
| Régua degenerada | `app/escala/regua_defesa.py:51` |
| Limbo/âncora/dia sem grade | `app/ponto/management/commands/detectar_ausencias.py:190`, `:271`, `:510` |
| Par relâmpago; cluster | `app/ponto/management/commands/detectar_par_relampago.py:368`; `app/ponto/services/cluster_espurio.py:130` |
| Ausência colaborador | `app/ponto/services/ausencia.py:659` |
| Auditoria geofence | `app/ponto/services/geofence.py:339` |
| Justificativa rejeitada | `app/ponto/services/justificativa.py:291` |
| Bloqueio/anuência/horário | `app/ponto/services/triagem_batida.py:107`, `:336`, `:459` |
| Sem posto/férias | `app/ponto/utils.py:40`, `:103` |
| Bloqueio entrada/ping | `app/ponto/views.py:291`, `:3012` |
| Reemissão por pergunta já existente | `app/chamados/reconciliador.py:910`, `:954` |
| Pais de disputa | `app/chamados/services/disputa_emissao.py:568`, `:1013`, `:1240` |
| Smoke, código de comando incluído pelo censo | `app/chamados/management/commands/smoke_chamados.py:123` |

Os pais de disputa são casos condicionais, pois seus fluxos emitem perguntas depois, sujeitas às guardas/dedup; a âncora emite CONVERSA. (`app/chamados/services/disputa_emissao.py:1044`, `:1052`, `:1272`)

**Script final efetivamente executado**, ampliando o censo nativo com chamadas sem filtro por receptor para preservar aliases (`_Ch9`, `_ChG`, etc.). A saída é conjunto de candidatos AST; classificação semântica e efeitos transitivos foram lidos à parte. O censo nativo documenta explicitamente inferências e balde não atribuível. (`app/core/censo_escritas.py:1`)

```python
import ast, pathlib, sys
sys.path.insert(0, 'app')
from core.censo_escritas import varrer_arvore, _e_codigo_de_producao
alvos = {'ChamadoColaborador', 'PerguntaDisputa'}
for r in varrer_arvore('app'):
    if r['entidade'] in alvos:
        print('ESCRITA', 'app/'+r['arquivo'], r['linha'],
              r['entidade'], r['op'], r['expr'])

portas = {
    'abrir', 'criar', 'reabrir', '_renascer_por_premissa_viva',
    '_gravar_estado', 'carimbar', 'carimbar_identidade',
    'fechar_por_admin', 'encerrar_fantasma',
    'encerrar_por_fato_no_espelho', 'reapontar_resolvedora',
    'rechavear_data_evento', 'validar_pergunta',
    'lavrar_veredito', 'salvar_e_emitir', 'trancar_periodo',
    'registrar_batida', 'anotar', 'retratar_batida',
}
campos = {
    'status_local', 'materializada_em', 'via_resolucao', 'validada_em',
    'chamado_destino', 'chamado_destino_id', 'data_evento',
    'celula', 'celula_id',
}

class Censo(ast.NodeVisitor):
    def __init__(self, p):
        self.p, self.pilha = p, []

    def visit_FunctionDef(self, n):
        self.pilha.append(n.name)
        self.generic_visit(n)
        self.pilha.pop()

    visit_AsyncFunctionDef = visit_FunctionDef

    def visit_Call(self, n):
        f = n.func
        nome = f.attr if isinstance(f, ast.Attribute) else getattr(f, 'id', '')
        if nome in portas:
            print('PORTA', self.p, n.lineno,
                  '.'.join(self.pilha), ast.unparse(n)[:220])
        self.generic_visit(n)

    def visit_Assign(self, n):
        for a in n.targets:
            if isinstance(a, ast.Attribute) and a.attr in campos:
                print('ATRIBUICAO', self.p, n.lineno,
                      '.'.join(self.pilha), ast.unparse(n)[:220])
        self.generic_visit(n)

for p in sorted(pathlib.Path('app').rglob('*.py')):
    if not _e_codigo_de_producao(str(p.relative_to('app'))):
        continue
    try:
        arv = ast.parse(p.read_text())
    except (SyntaxError, UnicodeError) as e:
        print('NAO_VERIFICADO', p, type(e).__name__)
        continue
    Censo(p).visit(arv)
```

Execução utilizada: `PYTHONDONTWRITEBYTECODE=1 python3 - <<'PY'` com esse corpo; nenhum arquivo criado/editado/apagado pelo script. `[nao verificado]` para a afirmação global sobre estado externo ao comando; o corpo usa só leituras, AST e impressão.

## 09/11 00:56 · sessao 866e10ad1838 · - · - · origem=Ronald (tmux)

**(1) Definição e diferenças**

O comando importa `VIVOS` em `app/chamados/management/commands/disparar_perguntas_competencia.py:77` e chama “vivo” o chamado com `status_local__in=VIVOS`, necessariamente ligado a célula cuja `data` está na janela solicitada; não filtra módulo em `:141`. `VIVOS=('aberto','em_analise','registrado')`; `COBRANCA=('aberto','em_analise')` é outro conjunto (`app/chamados/catalogo/motor.py:24-31`).

A autoridade registrada para chamado vivo é `_C1→VIVOS`; pergunta viva é `_C6→pergunta_viva`; redução da coleção é `_C15→fio_vivo` (`app/core/juizes.py:107-125`, `:261-275`). O anexo vivo `app/docs/JUIZES.md:54-57` remete aos conjuntos do motor; o registro explica expressamente que `VIVOS` não responde se existe cobrança/fio (`app/core/juizes.py:100-106`).

O comando usa `any(_viva(p) for p in ch.perguntas_que_resolvem.all())` (`.../disparar_perguntas_competencia.py:148`). `_viva` é `models.pergunta_viva`, reexportação direta de `juizes.pergunta_viva` (`app/chamados/models.py:4060-4063`). Logo, **a decisão por pergunta é a mesma autoridade**, sem filtro próprio de materializada/retratada/encerrada nesse primeiro teste. A redução é expressão equivalente ao corpo de `fio_vivo`, mas está copiada em vez de chamar `_C15` (`app/chamados/juizes.py:1253-1262`).

`pergunta_viva` memoiza e delega a `_pergunta_viva_calc` (`app/chamados/juizes.py:837-863`); degraus atuais:

- validada e dia trancado morrem (`:1153-1158`);
- via que vence resposta + materializada morre (`:1164-1165`);
- respondida com texto vive salvo decisão admin/concordância com luz (`:1167-1181`);
- identidade + materializada morre (`:1189-1191`);
- materializada cujo próprio marco acendeu morre (`:1196-1198`);
- célula acusadora vive; célula que absolve pode ser provisória (`:1200-1215`);
- fallback consulta **dia do turno**, janela viva e `classificar_falta` (`:1216-1226`);
- sem materialização vive; materializada depende de lastro (`:1227-1238`).

**DECISÃO D1 — universo/status:** `_C1` responde somente status, sem janela, FK de célula ou módulo; o comando restringe célula/janela, mas inclui `registrado` e todos os módulos (`motor.py:24-29`; comando `:141`). Isso é diferente de “cobrança viva”.

**DECISÃO D2 — coleção:** pergunta viva sob outro destino, mesmo dia/fato, não impede o primeiro teste do comando; ele olha `perguntas_que_resolvem` daquele chamado. O vigia legado olha todas as perguntas da célula (`comando:147-148`; `vigia_fio.py:398-417`). [nao verificado]

**DECISÃO D3 — alvo emitível não é todo chamado sem pergunta viva:** o comando exclui parcial na lista, ausência de motivo vencido, desligado, `_ja_perguntou` e qualquer linha de identidade do fato (`comando:154-219`).

**DECISÃO D4 — duas identidades:** `_ja_perguntou` consulta motivo/data/conversa=False **dentro da disputa encontrada** e decide via `fio_vivo`; `linhas_dos_fatos` consulta **coluna colaborador**, motivo/data/**instância_marco**, conversa=False e exclui `VIAS_FORA_DO_FATO`, independentemente de vida (`disputa_emissao.py:1314-1323`; `models.py:2729-2753`). O comando escolhe `DisputaSupervisao.objects.filter(colaborador=colab).first()` antes do primeiro teste (`comando:172-175`). [nao verificado]

**DECISÃO D5 — retratada/duplicata:** `linhas_dos_fatos` exclui ambas via fonte `VIAS_FORA_DO_FATO=('duplicata','motivo_retratado')`; não exclui pergunta apenas por ser validada/materializada/encerrada. Portanto `[BARRADO] FIO DECIDIDO` é existência do fato, não teste de “validada” (`models.py:2391`, `:2752-2753`; comando `:215-228`). [nao verificado]

**DECISÃO D6 — regra de emissão célula versus fio:** `motivos_a_perguntar` consulta `dia_sem_trabalho`, ativo, futuro, veredito acusador e parcial (`fabrica_da_celula.py:89-104`); ramo fio mudo chama diretamente `motivos_pela_ata` e teto, sem chamar essas guardas integralmente (`comando:154-159`). O rótulo “dia ACUSADO pela lei” não equivale a `cel.veredito in VEREDITOS_ACUSAM`: o ramo do fio não faz essa comparação (`comando:158`, `:221`).

**DECISÃO D7 — tempo:** fio usa `ch.celula.data` como dia do turno e teto por instante, incluindo hoje se o marco venceu; ramo célula do vigia exclui hoje e ontem enquanto candidato do vigia de hora (`comando:91-112`, `:150-159`). `vencidos_no_instante` preserva exceção por data: passado sem marcos devolve `None`; hoje/futuro sem marcos devolve conjunto vazio (`fabrica_da_celula.py:15-31`).

**(3) Leitores e unidade**

| Leitor | Universo/unidade/significado |
|---|---|
| Comando `[EMITE]`, `ALVOS`, empresas | Lista de **tuplas distintas colaborador/dia/motivo**, não contagem bruta de chamados. Escolhe primeiro motivo ainda não visto por chamado; imprime total `_mudos`, depois `ALVOS` após `--limite`. `disparar_perguntas_competencia.py:158-219`, `:220-245`. |
| Comando `FABRICA-POR-EVENTO` | **Perguntas/motivos do ramo célula** que o evento deixou de emitir, mais quantidade de células distintas; não é contador de fio mudo. `:102-113`, `:237-243`. |
| `--alarme` | Acusa célula e fio mudo; ramo fio usa lista já cortada por limite; nada escreve. `:235`, `:253-260`. |
| Auditor F1 | **Chamados** em todos VIVOS, sem qualquer pergunta viva **e sem qualquer resposta textual**; exclui desligado, dia inapúravel e dia absolvido por `classificar_falta`; dia é primeira `p.data_evento` disponível, só depois célula. Sem janela explícita. `auditar_invariantes_chamados.py:1626-1684`. |
| Auditor F1t | Piso excluído do F1: pergunta daquele motivo/dia por disputa.colaborador, via não vazia, morta pelo juiz e célula acusadora. Não usa `linhas_dos_fatos`, não exclui retratada/conversa por identidade. `:1706-1720`. |
| `vigia_fio.censo` | **Células/dias**, universo de chamados VIVOS com célula, somente `FURO_MODULES`; exige célula acusadora, lê todas perguntas da célula, separa hoje/futuro, e retira quatro baldes com dono/ação. Sem janela de competência. `vigia_fio.py:334-379`, `:398-449`. `fio_morto_cobrando` é subconjunto em COBRANCA (`:350-351`, `:451-452`). |
| `vigia_fio.censo_alcance` | **Dias retidos na folha**, nas competências abertas e empresas visíveis; verifica porta na fila/app pelo C17. Não exige chamado vivo; hoje/futuro e quatro baldes de lei saem do contador. `vigia_fio.py:542-583`, `:635-699`. Código distingue explicitamente este contador do legado (`:493-499`). |
| `vigiar_fio_morto` | Mede os dois censos no mesmo `hoje`, captura um instante para data/carimbo; grava alcance e legado. Esperado 0; alcance **ainda fora de ALARMA_EM_0**. `vigiar_fio_morto.py:82-96`, `:159-178`, `:210-225`. |
| `bin/fio_estado.py` | Lê carimbo; manchete **fio_morto_alcance**, antigo só “universo antigo/regressão”. Compara esperado do próprio carimbo e idade do instante `medido_em`; não recalcula universo. `:121-162`, `:175-190`. |
| `bin/gerar_estado.py` | Incorpora essa composição por `ler/linha`. `:253-261`. |
| `bin/placar_code.sh` | De `logs/fio_mudo.log`, raspa **somente FABRICA-POR-EVENTO**, exigindo mtime posterior à meia-noite civil do host. Rótulo `perguntas_que_o_julgamento_nao_fez`, fora TOTAL. Não raspa `[EMITE] FIO MUDO`. `:388-390`. |
| Placar estrutural de crons | Conta **nomes de crons com papel juiz**, não chamados/perguntas/dias; encadeados contam. `config/crons.py:2343-2352`; `bin/placar_code.sh:482-486`. |
| Matriz contrato 20 | Consome lista `JUIZES_POR_VARREDURA` e `PENDENTES_CHAMADO` como exceções à célula juiz/chamado; não lê número do fio. `contratos_estruturais.py:261-270`. |
| Corrente crons | Executa comando com `--comp-corrente --apply`, após `apurar_furos_diarios`, log fio_mudo. Comentário reconhece escrita do fio sem evento. `config/crons.py:1516-1517`, `:2039-2043`. |
| `vigia_loop.escritas_por_cron`/placar | **Linhas LogAuditoria** por ator `sistema:<cron>` na janela móvel de 7×24h; objetos distintos modelo/id e regravados >3. Não conta perguntas vivas e omite escrita sem trilha. `vigia_loop.py:61-99`; `bin/placar_code.sh:201-209`. |
| `pergunta_no_app.medir` | **Chamados** COBRANCA, CALADOS_POR_ISENCAO, colaborador ativo; usa `_dia` do catálogo/contexto, exclui hoje/futuro; conta causas de ausência no app, inclusive causas de silêncio legítimo/admin. `pergunta_no_app.py:56-97`, `:122-152`. |
| Command de mesmo nome | Publica contador e causa/dono. `chamados_vivos_sem_pergunta_no_app.py:12-21`. |
| `pergunta_no_app.do_dia` / copiloto | Chamados VIVOS no dia apurado pelo contexto; lista perguntas app e retenção. `pergunta_no_app.py:155-172`; `ponto/services/dia_do_colab.py:99-109`; `cobrar_dia.py:43-50`. |
| Tela atendimento | `tem_pergunta_viva` exige **viva E ação possível**, mais estreito que comando; formulário furo sem flag só em aberto/em_analise. `fio.py:279-287`; `_card_atendimento.html:203-205`. |
| App/ficha | Perguntas do app passam `q_vivas` generoso e depois juiz exato, universo pessoa e qualquer disputa. `pergunta_no_app.py:42-53`; `api/views.py:2567-2568`; `colaboradores/services/ficha.py:166`. |
| Instrumento reparo | Lê censo legado para antes/depois do passivo. `materializar_passivo_validado.py:201-203`. |

`q_vivas` promete **continência, não igualdade**; quem usa só SQL não mede pergunta viva exatamente (`models.py:3326-3333`). Diferenças F1/comando/vigia acima são decisões concretas de universo, unidade, dia e exclusão; nenhum desses números deve ser intercambiado sem escolher qual pergunta responder.

**(6) Condição de saída e C11**

Literal: **“sai quando o fio mudo for medido e o C11 pousar”** (`app/docs/CORTES.md:305`; `bin/tests/test_papel_prazo_nao_deriva.sh:307`). `LEIS.md:315` só lista o identificador; não transcreve nem define C11.

A exceção implementada contém motivo e saída; selo exige campos e exige cron ainda na lista, mas **não testa se fio foi medido ou se C11 pousou** (`bin/tests/test_papel_prazo_nao_deriva.sh:300-326`).

**C11 foi esclarecido posteriormente no registro de aval:** `app/docs/PROMPTS.md:1875` registra 05/11 16:0x **CONTRATO-20-DECISOES**: “**3 FIO MUDO C11=(i), evento na abertura de batida_ausente + 5 atos de perda, universo 494, padrao=evento**”. Antecedente: `PROMPTS.md:1519` e `RELATO.md:1829` registram medir as 12 células: sem furo fecha por lastro; com furo evento de abertura. Essa evidência documental impede dizer genericamente “não existe definição de C11”.

C11 desse aval não deve ser confundido com `_C11` ausência (`core/juizes.py:117`, `:271`) ou assinatura C11 folga sem porta (`escala/services/cadastro_realidade.py:119-126`). São identificadores de perguntas distintas.

**Estado da cópia:** comando continua emitindo no fio (`disparar...:282-295`), cron continua juiz (`config/crons.py:2039-2043`), exceção continua presente (`test_papel...:302-309`): saída estrutural **não cumprida nesta cópia**. Medida/pouso efetivo em produção e números históricos documentados: **[nao verificado]**, porque dado fornecido não traz sonda/log. O código já mede via comando/F1/vigia, mas com universos distintos; não prova execução ou dimensão real.

**Script AST adicional que usei para identificar consumidores diretos** (inventário principal de escritores ficou com outro agente):

```python
import ast
import pathlib

for root in ("app", "bin"):
    for p in sorted(pathlib.Path(root).rglob("*.py")):
        if any(x in p.parts for x in ("tests", "migrations", "logs")):
            continue
        if p.name.startswith("test_"):
            continue
        try:
            tree = ast.parse(p.read_text())
        except (SyntaxError, UnicodeError):
            continue
        aliases = {}
        for node in ast.walk(tree):
            if isinstance(node, ast.ImportFrom) and node.module in (
                "chamados.services.vigia_fio",
                "chamados.services.pergunta_no_app",
                "config.crons",
                "fio_estado",
            ):
                for item in node.names:
                    aliases[item.asname or item.name] = (
                        node.module, item.name
                    )
        for node in ast.walk(tree):
            if (
                isinstance(node, ast.Call)
                and isinstance(node.func, ast.Name)
                and node.func.id in aliases
            ):
                print(
                    f"{p}:{node.lineno}: "
                    f"{aliases[node.func.id]}"
                )
```


## 09/11 00:56 · sessao 9f546eba1d3b · - · - · origem=Ronald (tmux)

**Q4 — ramo escritor**

- O comando seleciona `ChamadoColaborador.status_local__in=VIVOS`, com célula na janela; descarta chamado com alguma pergunta viva ligada, parcial só lista, sem motivo vencido da ata, colaborador inativo, pergunta viva na disputa encontrada e fato já existente. O alvo é `(colab, ch.celula.data, motivo)`, deduplicado nessa chave; portanto a seleção ocorre antes das travas da escrita. `app/chamados/management/commands/disparar_perguntas_competencia.py:141-219`.
- Escreve por `fabrica_da_celula.perguntar(... origem='disparo_competencia', origem_carimbo='emissor fio mudo')`; `--apply` envolve o lote inteiro em `transaction.atomic()`, com push silenciado por padrão. Dry executa a mesma escrita e força rollback com `RuntimeError`; `--alarme` retorna antes dessa escrita. `app/chamados/management/commands/disparar_perguntas_competencia.py:253-260`, `:270-295`, `:306-317`.
- `perguntar` primeiro tenta derrubar carimbos sem lastro de perguntas não conversacionais do motivo/dia; depois chama `criar_ou_atualizar_disputa(colab,[motivo],{motivo:{'data':dia.isoformat()}})` e, se houver disputa, `gerar_perguntas_disputa`. A primeira porta já gera perguntas tanto no nascimento como no reuso, portanto a chamada posterior repete a geração sob dedup. `app/chamados/services/fabrica_da_celula.py:139-152`; `app/chamados/services/disputa_emissao.py:662-688`.
- Pode gravar carimbo desfeito, reabertura de disputa fechada, disputa nova/motivos/contexto agregado, chamado-pai, perguntas e costuras/trilhas associadas; cada efeito depende de seus gates. Carimbo desfeito e reabertura: `app/chamados/services/relavra_via.py:219-260`; disputa/contexto/pai: `app/chamados/services/disputa_emissao.py:535-603`; geração/costura: `:443-469`.
- Data inicial: **dia do turno da célula**, `ch.celula.data`, e não data civil do pedido. A geração lê `contexto[motivo]['data']`, usa `dia_do_fato` do destino e passa `dia_do_turno_da_celula(_dia_p)` à porta. `app/chamados/management/commands/disparar_perguntas_competencia.py:150`; `app/chamados/services/fabrica_da_celula.py:149`; `app/chamados/services/disputa_emissao.py:371-375`, `:230-254`, `:443-451`; `app/ponto/turnos.py:2929-2933`.
- A porta de nascimento ainda deriva o dia real do episódio, quando houver batida de origem; reuso com dia diferente passa pelo rechaveamento, cujo veto lê competência exportada/desconhecida dos dias antigo e novo. Logo, **não afirmar que nenhuma parte da cadeia consulta exportada**. `app/chamados/models.py:2860-2876`, `:2925-2955`; `app/chamados/services/fio.py:1039-1058`.
- O caminho comum do fio mudo não consulta `limbo_vai_a_pauta` nem tem guarda própria de tranca/exportada; usa diretamente `perguntar`. Já o caminho da célula consulta limbo entregue e cria Pauta DP em vez da pergunta. `app/chamados/management/commands/disparar_perguntas_competencia.py:270-283`; `app/chamados/services/fabrica_da_celula.py:175-186`, `:190-198`.
- A tranca entra na leitura de vitalidade de perguntas existentes por C6: `validada_em` mata, depois `dia_da_pergunta_trancado` mata. A seleção sem pergunta existente não faz essa consulta sobre uma pergunta nascente. `app/chamados/juizes.py:1152-1158`; `app/chamados/management/commands/disparar_perguntas_competencia.py:148-219`. Existência desse caso em produção: `[nao verificado]`.

**Travas do ramo:**

1. `desfazer_carimbo_sem_lastro`: atomic por pergunta; `trava_na_ordem(C, disputas=[D], perguntas=[P])`, releitura e juiz antes de descarimbar. `app/chamados/services/relavra_via.py:219-246`.
2. `_abrir_ou_agregar_disputa`: atomic; trava universo do colaborador — C, todas disputas abertas, pais e chamados vivos — antes de gravar. `app/chamados/services/disputa_emissao.py:521-530`; implementação da ordem: `app/chamados/models.py:2627-2692`.
3. `PerguntaDisputa._nascer`: atomic sob contrato de saída e trava C antes de dedup/nascimento; conversa também trava C. `app/chamados/models.py:2817-2820`, `:2839-2843`.
4. Não identifiquei escrita D/Ch/P desse ramo anterior à aquisição de C nessas portas. Isso **não significa** que todo `save()` de pergunta tenha selo `exigir_colaboradores_travados`: `PerguntaDisputa.save` só envolve `super().save` em atomic. `app/chamados/models.py:3403-3413`. O selo explícito aparece em portas específicas, como `descarimbar_em_lote`: `:3247-3255`.
5. O atomic externo do comando percorre múltiplos colaboradores sem pré-travar o conjunto por PK: a queryset não tem `order_by`, `_mudos` preserva sua iteração e `_rodar` percorre a lista. Isso difere da regra documentada de travar todos os C da transação em ordem antes de D/Ch/P. `app/chamados/management/commands/disparar_perguntas_competencia.py:141-147`, `:218-219`, `:273-295`; `app/chamados/models.py:2652-2666`. É diferença estrutural de ordem, não caminho identificado sem **a** trava do dono.

**Q5 — eventos e encaixes existentes**

| Ponto | O que existe e o que trava |
|---|---|
| Pergunta salva após resposta/validação/carimbo/retrato/reapontamento | `_outbox_post_save_pergunta`, síncrono, exceto nascimento/raw; só campos em `CAMPOS_QUE_ACORDAM` ou save completo. `app/chamados/signals.py:237-244`; campos: `app/chamados/models.py:3210-3212`. Save e evento entram juntos no atomic de `PerguntaDisputa.save`; o receiver não adquire C/D/Ch/P. `:3403-3413`, `:3221-3238`. |
| Destino/dia/célula mudados | O outbox captura **estado atual** da pergunta: FK célula, ou fallback `(disputa.colaborador,data_evento)`; não captura sozinho âncora antiga. `app/chamados/models.py:3215-3238`. `reapontar_resolvedora` salva destino e lavra, tomando C e trava do destino pela costura; não acorda explicitamente a célula antiga. `app/chamados/services/fio.py:873-917`. `rechavear_data_evento` toma C/D/P, salva dia/célula e acorda também a célula velha explicitamente. `:989-1028`. |
| Pergunta salva: beco de disputa | `_becos_post_save_pergunta` agenda `on_commit`, best-effort; chama `fechar_beco_da_disputa`. `app/chamados/signals.py:214-225`. Só é beco com pai terminal, nenhuma pergunta visível e C7 permitindo fechar. `app/chamados/services/becos.py:25-33`. O ato usa atomic e trava C/D/todos Ch envolvidos/universo aberto antes de resolver e fechar. `:41-57`. Não cobre genericamente chamado vivo sem pergunta viva, pois o pai precisa estar terminal. |
| Chamado nasce ou muda status | `_cartorio_post_save_chamado` chama `julgar_celula` sincronicamente, exceto raw/status igual/em_julgamento. `app/chamados/signals.py:178-203`. `_gravar_estado` já trava universo C/D/Ch antes de save e cascatas. `app/chamados/models.py:280-296`. A fábrica roda apenas para as datas que este ato julgou, portanto o signal não garante emissão se o julgamento pulou por impressão estável. `app/ponto/services/cartorio.py:981-997`. |
| Chamado de turno nasce em `abrir` / `criar` | Emitem `EventoCelula` no mesmo atomic se módulo de turno e célula presente. `app/chamados/models.py:971-980`, `:1093-1099`. `abrir` toma C pela porta: `:905`; `criar` deve ser examinado no inventário de escritores, pois o fragmento da emissão não toma C por conta própria. |
| Chamado reabre | `_gravar_estado` provoca signal do cartório, depois `ressuscitar_perguntas`; não há chamada direta à fábrica na reabertura. `app/chamados/models.py:1429-1442`. Não nasce pergunta nova necessariamente; universo de ressuscitação pode ser vazio. `:1444-1456`. |
| Consumidor da célula | Rejunta células, retrata perguntas absolvidas, retrata chamados, premissa morta, encerra órfãs e fecha cobrança com lastro. **Não chama fábrica/perguntar.** `app/chamados/reconciliador.py:1363-1383`. O consumidor não toma C/D por conta própria: portas travam cada ato; atomic curto só marca eventos. `:1323-1327`, `:1387-1397`. Exclusão da execução por advisory lock está no comando. `app/chamados/management/commands/consumir_eventos_celula.py:17-23`. Frequência é um minuto, portanto outbox não significa executar no mesmo instante do save. `app/config/crons.py:394`. |
| Fecho por lastro no consumidor | Só `batida_ausente` em `qs_na_fila`, marcos da célula acesos; atomic une `reconciliar` e nota. `app/ponto/services/lastro.py:364-403`. `reconciliar` chama gates premissa e pergunta viva; releitura dentro do atomic toma C/D/Ch universo. `app/chamados/models.py:650-655`, `:682-688`, `:707-712`. Não é regra genérica “sem pergunta viva → fecha”. |
| Perguntas absolvidas pela célula | Atomic por pergunta, trava C e exige selo; carimba por C6. Depois pode fechar disputa via C7. `app/chamados/reconciliador.py:592-614`. |
| Disputa fecha | S88 tenta fechar pai por `reconciliar()` e nota no mesmo atomic; S144 materializa pendentes pela porta. Ambos best-effort. `app/chamados/signals.py:18-49`, `:147-165`. |

A função `emitir_evento_celula` apenas cria a linha do outbox; não julga, fecha nem emite pergunta diretamente. `app/ponto/portas/celula.py:882-891`.

**Perguntas de lei possíveis para consolidação, sem resposta proposta**

1. Quando a última pergunta deixa de viver e o dia continua acusando, o efeito exigido é fechar chamado, reemitir pergunta ou encaminhar decisão ao admin? Dependem: comando `:148-219`, consumidor `reconciliador.py:1363-1383`, gate `models.py:717-725`.
2. Linha do fato existente, sem pergunta viva, deve continuar “fio decidido” barrado ou pode ganhar nova pergunta/ressuscitação? O código barra toda linha que ainda pertence ao fato; o texto impresso a chama decidida mesmo sem testar `validada_em` nesse gate. `disparar_perguntas_competencia.py:201-228`; `models.py:2728-2737`.
3. O fio mudo em célula `trabalha=None` e competência entregue segue perguntando ou deve seguir a Pauta DP do caminho da célula? Dependem: comando `:270-283`; fábrica `:181-198`.
4. O evento de nascimento/reabertura de chamado precisa produzir pergunta independentemente de uma nova lavra da célula, ou vale depender do julgamento efetivo? Dependem: signals `:187-203`; cartório `:981-997`; reabertura `models.py:1429-1442`.
5. Dia trancado sem pergunta anterior pode receber linha nova que C6 mantém morta por leitura? Dependem: comando `:148-219`; fábrica `:139-151`; C6 `juizes.py:1152-1158`. Ocorrência em produção `[nao verificado]`.

Os pontos 2–5 devem ser filtrados pela leitura integral das leis pelo agente responsável; aqui são divergências entre caminhos, sem determinar se já existe aval suficiente.

## 09/11 00:48 · sessao d2b417eeeb67 · FIO-E0-V4-E1-FILA · sonda · origem=Code

Transcrição literal do dado fornecido [nao verificado]:

```text
REATARd universo colabs= 248 perguntas= 3359 erros= 0
REATARd HIST reapontaria a TERMINAL= 0 a vivo= 11
REATARd c1 lavraria= 0 julgar_celula escreveria= 0
REATARd c2 abrir chamado= 33 criaria novo= 33 veto na porta= 0
REATARd c2 abrir achou TERMINAL e tentou RENASCER= 0 status= []
REATARd RENASCER escreveria (guardas passaram)= 0 vetado pelas guardas= 0
REATARd pares (pergunta,chamado) que renasceriam (ate 20)= []
FIM sonda_v22d_acervo
```

1. **Não escreve quando `sistema` já existe.** A implementação retorna o usuário por `User.objects.get_or_create(username='sistema')[0]`; a criação ocorre quando ele não existe. A docstring que anuncia `DoesNotExist` diverge dessa implementação. — app/core/utils.py:15–18.

2. **Não, considerando escrita persistida no banco e retorno booleano normal.** `carimbar` aplica o carimbo **em memória**, consulta `pergunta_viva` e pode desfazê-lo e retornar `False`, mas essa decisão ocorre **antes** do `save`. — app/chamados/models.py:3087–3091, 3119–3135, 3142.

   Depois dessa gravação, `lavrar_sem_turno_aberto` consulta `p.disputa.pode_fechar()` para decidir o fechamento da disputa; o resultado dessa consulta não muda seu retorno: a função termina com `True`. Portanto, essa leitura posterior não sustenta a hipótese de “lavraria” contar um caso que retornaria `False`. — app/chamados/reconciliador.py:405–414.

   Quando a lavra retorna `False`, `reatar_pergunta_orfa` ainda tenta o caminho de reemissão `(c2)`. — app/chamados/reconciliador.py:1615–1618.

## 09/11 00:45 · sessao b8280ae407bb · FIO-E0-V4-E1-FILA · sonda · origem=Code

Transcrição literal do dado fornecido; execução da sonda sem verificação independente. [nao verificado]

```text
ERRO p= 48633 _Lavra 
ERRO p= 48634 _Lavra 
ERRO p= 48637 _Lavra 
ERRO p= 48638 _Lavra 
ERRO p= 48641 _Lavra 
REATARc universo colabs= 248 perguntas= 3359 erros= 52
REATARc HIST reapontaria a TERMINAL= 0 a vivo= 11
REATARc c1 lavraria= 0 julgar_celula escreveria= 0
REATARc c2 abrir chamado= 33 criaria novo= 33 veto na porta= 0
REATARc c2 abrir achou TERMINAL e tentou RENASCER= 0 status= []
REATARc RENASCER escreveria (guardas passaram)= 0 vetado pelas guardas= 0
REATARc pares (pergunta,chamado) que renasceriam (ate 20)= []
FIM sonda_v22c_acervo
```

1. **Sim, existe dependência no código que pode mudar o caminho do c2.** `julgar_celula` chama `julgar_colab`; o corpo lavra e persiste a célula antes de a fábrica chamar a emissão de perguntas, que pode alcançar `reatar_pergunta_orfa` e o c2 ainda dentro do julgamento. (app/ponto/services/cartorio.py:1989, app/ponto/services/cartorio.py:1802, app/ponto/services/cartorio.py:1822, app/ponto/services/cartorio.py:997; app/chamados/services/fabrica_da_celula.py:186, app/chamados/services/fabrica_da_celula.py:151; app/chamados/services/disputa_emissao.py:464; app/chamados/reconciliador.py:1617)

   Nesse caminho, o c2 consulta `protestos` e pode retornar antes de `abrir`; também consulta `ata` e `julgada_em` para o veto `parcial_so_lista`. Portanto, desfazer as escritas pode mudar essas decisões. (app/chamados/reconciliador.py:867, app/chamados/reconciliador.py:869, app/chamados/reconciliador.py:878, app/chamados/reconciliador.py:882) Que isso tenha mudado algum caso concreto da sonda: [nao verificado].

2. **Não há nova guarda de veto depois da escrita que efetiva a reabertura.** `_renascer_por_premissa_viva` verifica situação, cascata, tranca e régua antes de chamar `reabrir`; esta consulta `recusa_da_reabertura` antes de `_gravar_estado`. Depois da gravação, ressuscita perguntas e retorna `True`; o renascimento anexa a ocorrência e retorna `True`. (app/chamados/models.py:1005, app/chamados/models.py:1019, app/chamados/models.py:1390, app/chamados/models.py:1432, app/chamados/models.py:1441, app/chamados/models.py:1022)

   **Há uma ressalva para “primeira escrita”: recusas também podem escrever trilha ou redirecionar perguntas e retornar `False`.** Assim, interceptar qualquer escrita, sem distinguir sua finalidade, não prova renascimento; o veto já foi decidido antes desses efeitos. (app/chamados/models.py:1017, app/chamados/models.py:1396, app/chamados/models.py:1402, app/chamados/models.py:1414, app/chamados/models.py:1426) Como a sonda distingue essas escritas: [nao verificado].

3. **Não há escrita persistente do c1 antes da chamada a `get_user_sistema`.** Antes dela estão as guardas; `p.carimbar` e o eventual fechamento da disputa vêm depois. (app/chamados/reconciliador.py:362, app/chamados/reconciliador.py:371, app/chamados/reconciliador.py:398, app/chamados/reconciliador.py:400, app/chamados/reconciliador.py:405, app/chamados/reconciliador.py:412) A própria `get_user_sistema` usa `get_or_create`, podendo escrever durante a chamada. (app/core/utils.py:18)

## 09/11 00:40 · sessao d3b51dece676 · FIO-E0-V4-E1-FILA · sonda · origem=Code

Transcrição literal do trecho fornecido; execução e autenticidade [nao verificado]:

```text
sonda_leitura: sonda_v22b_acervo.py [a670b468a300454b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

2026-11-09 00:37:11,564 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=4929, colab=42) — pergunta criada SEM destino.
2026-11-09 00:37:11,594 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=4929, colab=42) — pergunta criada SEM destino.
2026-11-09 00:37:18,056 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'saida_sem_entrada' (disputa=7952, colab=364) — pergunta criada SEM destino.
2026-11-09 00:37:18,063 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=7952, colab=364) — pergunta criada SEM destino.
2026-11-09 00:37:20,948 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_saida' (disputa=5362, colab=503) — pergunta criada SEM destino.
2026-11-09 00:37:26,476 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_saida' (disputa=7721, colab=584) — pergunta criada SEM destino.
2026-11-09 00:37:26,485 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_volta' (disputa=7721, colab=584) — pergunta criada SEM destino.
2026-11-09 00:37:26,493 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=7721, colab=584) — pergunta criada SEM destino.
2026-11-09 00:37:39,118 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'saida_sem_entrada' (disputa=1651, colab=841) — pergunta criada SEM destino.
2026-11-09 00:37:39,129 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_saida' (disputa=1651, colab=841) — pergunta criada SEM destino.
2026-11-09 00:37:39,140 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_volta' (disputa=1651, colab=841) — pergunta criada SEM destino.
2026-11-09 00:37:39,154 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=1651, colab=841) — pergunta criada SEM destino.
2026-11-09 00:37:39,167 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'saida_sem_entrada' (disputa=4984, colab=844) — pergunta criada SEM destino.
2026-11-09 00:37:39,178 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_saida' (disputa=4984, colab=844) — pergunta criada SEM destino.
2026-11-09 00:37:39,187 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_volta' (disputa=4984, colab=844) — pergunta criada SEM destino.
2026-11-09 00:37:39,197 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=4984, colab=844) — pergunta criada SEM destino.
2026-11-09 00:37:39,410 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=1397, colab=865) — pergunta criada SEM destino.
2026-11-09 00:37:45,002 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'saida_sem_entrada' (disputa=4542, colab=917) — pergunta criada SEM destino.
2026-11-09 00:37:45,017 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=4542, colab=917) — pergunta criada SEM destino.
REATAR universo colabs= 248 perguntas= 3359 erros= 0
REATAR reapontaria a Chamado TERMINAL (HIST)= 0 pks(ate 20)= []
REATAR reapontaria a Chamado vivo (derivado)= 11
REATAR reapontaria a Chamado de OUTRO colab= 0
REATAR chegaria ao lavrar (c1)= 3344 ao reemitir (c2, renascer possivel)= 3344 pks(ate 20)= [51431, 29870, 35204, 51830, 52437, 52500, 54837, 54898, 36476, 36477, 36484, 36485, 36486, 36487, 36488, 36489, 36490, 36491, 36492, 36493]
FIM sonda_v22b_acervo
```

**Cobertura integral das interceptações: [nao verificado].** O fonte `sonda_v22b_acervo.py` não acompanha o dado e não foi localizado na cópia; a saída não informa quais atributos foram substituídos nem seus retornos. Portanto, não consigo identificar uma escrita como comprovadamente **não interceptada**. [nao verificado]

A conferência do código mostra os seguintes pontos relevantes:

- `perguntas_orfas_reatavies` apenas monta o filtro; `reconciliar_perguntas_orfas_colab` percorre esse conjunto, chama `reatar_pergunta_orfa` e registra exceções. (`app/chamados/reconciliador.py:1495`, `app/chamados/reconciliador.py:1517`, `app/chamados/reconciliador.py:1522`, `app/chamados/reconciliador.py:1525`)
- **Costura:** `reapontar_resolvedora` salva a pergunta e pode retratar outras perguntas por colisão; essa retratação chega ao `save` de `carimbar_identidade`. Substituir a função inteira impede a execução desses caminhos internos. (`app/chamados/services/fio.py:913`, `app/chamados/services/fio.py:916`, `app/chamados/services/fio.py:1324`, `app/chamados/services/fio.py:1342`, `app/chamados/models.py:3180`)
- **c1:** pode obter/criar o usuário sistema, carimbar a pergunta e fechar a disputa. Substituir `lavrar_sem_turno_aberto` inteira impede esses caminhos internos. (`app/chamados/reconciliador.py:400`, `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:412`; escritas em `app/core/utils.py:18`, `app/chamados/models.py:3142`, `app/chamados/models.py:1934`)
- **c2:** pode criar ou renascer chamado e depois costurar a pergunta. Há também `cartorio.julgar_celula` durante a avaliação de `_ctx_c2`, **antes de entrar em `ChamadoColaborador.abrir`**. Interceptar somente `abrir` não bloqueia essa chamada; substituir `reemitir_chamado_do_dia` inteira bloqueia o caminho. (`app/chamados/reconciliador.py:702`, `app/chamados/reconciliador.py:910`, `app/chamados/reconciliador.py:920`, `app/chamados/reconciliador.py:954`, `app/chamados/models.py:922`, `app/chamados/models.py:971`)
- **Atenção ao HIST:** em `resolver_perguntas_por_chamado_resolvido(hist[0], get_user_sistema())`, o argumento `get_user_sistema()` é executado mesmo que o resolvedor tenha sido substituído. Ele contém `User.objects.get_or_create`, uma escrita potencial se o usuário não existir. Esse caminho depende de o reapontamento retornar `_ok=True`; sua interceptação pela sonda permanece [nao verificado]. (`app/chamados/reconciliador.py:1638`, `app/chamados/reconciliador.py:1639`, `app/chamados/reconciliador.py:1644`, `app/core/utils.py:18`)
- Os `save`s reais de pergunta também alcançam receivers: fechamento de beco após commit e emissão condicional de evento da célula. A emissão chega a `EventoCelula.objects.create`. (`app/chamados/signals.py:224`, `app/chamados/signals.py:225`, `app/chamados/signals.py:244`, `app/chamados/models.py:3238`, `app/ponto/portas/celula.py:891`)

**Import local: sim, pega o atributo substituído**, desde que a substituição esteja em `chamados.services.fio.reapontar_resolvedora` quando o import executar. O vínculo local é obtido dentro de cada chamada de `reatar_pergunta_orfa`; c2 também faz seu próprio import local. A substituição efetivamente feita pela sonda permanece [nao verificado]. (`app/chamados/reconciliador.py:1546`, `app/chamados/reconciliador.py:791`)

## 09/11 00:39 · sessao 572f905bcef1 · BUG-DEPLOY-PORTAO · verificador · origem=Code

1. **Achado funcional: pendências simultâneas deixam as duas portas bloqueadas.** Com `PEND=1` e `PEND_MSG=1`, o deploy normal recusa pelo mensageria e recomenda `--restart`; este recusa pelo core e recomenda deploy normal. A orientação nova não resolve essa combinação. O caso 17 cobre apenas sua recusa no modo normal; o 22 cobre somente mensageria pendente. Ocorrência em produção: [nao verificado]. (`bin/deploy.sh:200`, `bin/deploy.sh:202`, `bin/deploy.sh:219`, `bin/deploy.sh:225`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:198`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:218`)

2. **Pergunta 1 — não identifiquei caminho para apply do core com `RESTART=1`.** `APLICAR_MIGRATE` e `RESTART` nascem zerados, neutralizando valores herdados. O único escritor posterior de `APLICAR_MIGRATE=1` está no `else` depois da recusa de restart; `falhou` encerra o script. Os ramos anteriores também encerram ou, no conferir, não armam o apply. (`bin/deploy.sh:61`, `bin/deploy.sh:70`, `bin/deploy.sh:92`, `bin/deploy.sh:206`, `bin/deploy.sh:208`, `bin/deploy.sh:211`, `bin/deploy.sh:215`, `bin/deploy.sh:219`, `bin/deploy.sh:230`, `bin/deploy.sh:312`)

   As flags não são combinadas: apenas `$1` seleciona o modo. `--restart --sem-migrate` interpreta `--sem-migrate` como motivo; na ordem inversa, seleciona somente `--sem-migrate`. Isso também vale para `--sem-sombra`, `--conferir` e `--reload-*`. `DEPLOY_SEM_SOMBRA` herdado dispensa a sombra, sem alterar a autorização de migrate. (`bin/deploy.sh:56`, `bin/deploy.sh:63`, `bin/deploy.sh:64`, `bin/deploy.sh:65`, `bin/deploy.sh:72`, `bin/deploy.sh:74`, `bin/deploy.sh:82`, `bin/deploy.sh:85`, `bin/deploy.sh:86`, `bin/deploy.sh:152`)

3. **Pergunta 2 — somente mensageria pendente conserva o caminho ao restart, com os demais portões verdes.** `RESTART=1` escapa da recusa por `PEND_MSG`; `PEND=0` não arma o apply do core. Depois dos portões, chega à checagem final de cron e ao `docker restart saas_core saas_ui mensageria`. O compose declara o migrate do mensageria no boot; sua execução real em produção é [nao verificado]. (`bin/deploy.sh:200`, `bin/deploy.sh:204`, `bin/deploy.sh:302`, `bin/deploy.sh:312`, `bin/deploy.sh:335`, `bin/deploy.sh:338`, `bin/deploy.sh:339`; `docker-compose.mensageria.yml:20`)

4. **Pergunta 3 — censo dos leitores e escritores:**

   | Nome | Escritores | Leitores/chamadas |
   |---|---|---|
   | `PEND` | `bin/deploy.sh:185`, `187` | `bin/deploy.sh:189`, `204`, `209`, `212`, `216`, `224` |
   | `APLICAR_MIGRATE` | `bin/deploy.sh:61`, `230` | `bin/deploy.sh:312` |
   | `RESTART` | `bin/deploy.sh:70`, `73` | `bin/deploy.sh:115`, `200`, `219`, `335`, `441` |
   | `recusa_restart_com_cron` | definição em `bin/deploy.sh:110` | chamadas em `bin/deploy.sh:117`, `338` |

   O leitor textual externo é `bin/contrato_deploy.sh`: reconhece o ramo `RESTART`, exige a chamada antes do restart e exige a função. Não exige migrate no restart nem checagem na 1c. O selo injeta `APLICAR_MIGRATE` pelo ambiente; o outro `PEND`, em `sondas.sh`, é um diretório independente. (`bin/contrato_deploy.sh:41`, `bin/contrato_deploy.sh:47`, `bin/contrato_deploy.sh:50`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:109`; `bin/sondas.sh:18`, `bin/sondas.sh:21`)

   Não identifiquei leitor decisório que dependa do significado antigo: `PEND` continua booleano do check, `APLICAR_MIGRATE` autoriza o apply e `RESTART` seleciona a publicação por restart. A incompatibilidade está nas recomendações recíprocas do achado 1 e na descrição obsoleta do caso 14, abaixo. (`bin/deploy.sh:184`, `bin/deploy.sh:200`, `bin/deploy.sh:225`, `bin/deploy.sh:230`, `bin/deploy.sh:312`, `bin/deploy.sh:335`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:187`)

5. **Pergunta 4 — remover a chamada da 1c não cria caminho sem recusa antes do restart.** O único `docker restart` continua imediatamente depois de `recusa_restart_com_cron`; há também a chamada inicial. Permanece a corrida entre a amostragem final e o restart: o envelope pode criar uma marca nesse intervalo, sem trava compartilhada. É o residual explicitamente preservado, não uma proteção de exclusão. (`bin/deploy.sh:117`, `bin/deploy.sh:309`, `bin/deploy.sh:338`, `bin/deploy.sh:339`; `bin/cron_run.sh:9`, `bin/cron_run.sh:10`)

6. **Pergunta 5 — os quatro casos mordem as regressões nomeadas, por inspeção das asserções, com limites:**

   - **11:** exige erro, zero apply e zero restart. Se o migrate voltar e gerar o cron falso, a recusa final não mascara o apply: o contador o reprova. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:86`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:111`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:174`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:175`)
   - **13:** cobra a mesma recusa sem cron injetado, cobrindo o restart com core pendente e portões configurados verdes. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:182`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:183`)
   - **22:** exige sucesso, zero apply do core e um restart quando somente mensageria pende. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:218`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:219`)
   - **23:** exige esses mesmos resultados sem pendências injetadas. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:222`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:223`)

   **Limites:** 11/13 não fixam a seção nem a mensagem da recusa; movê-la para depois dos portões, ainda antes do apply, preservaria suas asserções. 22/23 contam qualquer comando `restart`, sem conferir seus containers; o docker falso não executa boot. Portanto, não provam a aplicação real da migration do mensageria. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:94`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:120`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:175`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:183`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:219`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:223`)

7. **Pergunta 6 — há duas afirmações documentais falsas.** “Antes de qualquer portão” contradiz a ordem: árvore, cron, auth e sombra vêm antes da recusa nova. Já “antes do banco andar”, no sentido de impedir o apply do core, corresponde ao fluxo. (`bin/deploy.sh:101`, `bin/deploy.sh:117`, `bin/deploy.sh:145`, `bin/deploy.sh:162`, `bin/deploy.sh:221`, `bin/deploy.sh:224`, `bin/deploy.sh:313`)

   O caso 14 ficou obsoleto: ainda diz que a **1c** recusa o cron nascido na casca, mas agora sua pendência de core aborta na seção 1, antes de executar a casca que criaria a marca. A 1c já não contém essa recusa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:39`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:186`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:187`; `bin/deploy.sh:219`, `bin/deploy.sh:312`)

O selo comportamental não foi executado nesta revisão: ele cria e remove arquivos temporários. Resultado dinâmico dos casos: [nao verificado]. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:60`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:64`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:131`)

## 09/11 00:39 · sessao d2e43dd5157c · - · - · origem=Ronald (tmux)

Auditoria das fronteiras, sem assumir os patches da sonda:

- `reatar_pergunta_orfa` chama c1 em `app/chamados/reconciliador.py:1615`; c1 obtém usuário em `:400`, carimba em `:405` e fecha disputa em `:412`. Escritas internas: `app/core/utils.py:18` (`User.objects.get_or_create`), `app/chamados/models.py:3142` (`carimbar.save`), `app/chamados/models.py:1934` (`DisputaSupervisao.fechar.save`) e auditoria em `app/chamados/models.py:1935` → `app/core/observ.py:134`.
- c2 é chamado em `app/chamados/reconciliador.py:1617`; chama `ChamadoColaborador.abrir` em `:910` e `reapontar_resolvedora` em `:954`. O argumento `defaults` avalia `_ctx_c2` em `:920`, que pode executar `cartorio.julgar_celula` em `:702` **antes da chamada a `abrir`**. Interceptar c2 inteiro cobre esse caminho; interceptar apenas `abrir` não cobre a avaliação de `_ctx_c2`. (`app/chamados/reconciliador.py:702`, `:910`, `:920`)
- `abrir` pode criar chamado em `app/chamados/models.py:971`; no existente encerrado chama renascimento em `:922`, cuja execução pode reabrir em `:1019` e anexar nota em `:1022`. A nota grava em `app/chamados/services/nota.py:103` ou `:107`.
- HIST: após reapontamento aprovado, a expressão `resolver_perguntas_por_chamado_resolvido(hist[0], get_user_sistema())` avalia `get_user_sistema()` antes de chamar o resolvedor. Assim, **mesmo com o resolvedor substituído**, o `get_or_create` do usuário continua alcançável, salvo interceptação adicional ou retorno `_ok=False` no reapontamento. (`app/chamados/reconciliador.py:1638`, `:1639`, `:1644`; `app/core/utils.py:18`)
- Resolver histórico, quando executado: carimba em `app/chamados/services/disputa_emissao.py:1764` e `:1838`, pode reabrir pai em `:1789`, inserir nota em `:1791`, auditar em `:1841` e fechar disputa em `:1865`. Gravações correspondentes: `app/chamados/models.py:3142`, `app/chamados/services/nota.py:103`, `app/core/observ.py:134`, `app/chamados/models.py:1934`.
- `_derivar_chamado_destino` consulta candidatos e pode emitir WARNING; não contém gravação ORM no corpo lido. (`app/chamados/services/disputa_emissao.py:198`, `:226`, `:240`, `:254`)
- `_dia_fora_do_alcance_c` escreve somente memo na instância no corpo lido, em `app/chamados/reconciliador.py:748`.

Sem o código da sonda, quais desses pontos foram efetivamente interceptados permanece **[nao verificado]**.

## 09/11 00:38 · sessao 5900d8a4c6d7 · - · - · origem=Ronald (tmux)

- **Bloqueio circular quando ambos pendem:** modo normal recusa `PEND_MSG>0` e recomenda `--restart`; este recusa `PEND>0` e recomenda modo normal. Nenhum dos dois avança com `PEND=1/PEND_MSG=1`; `--conferir` apenas consulta. Referências: `bin/deploy.sh:200`, `bin/deploy.sh:202`, `bin/deploy.sh:219`, `bin/deploy.sh:225`, `bin/deploy.sh:234`. Ocorrência em produção: **[nao verificado]**. O caso 17 cobre apenas a recusa normal com ambos pendentes; o 22 cobre somente mensageria pendente: `bin/tests/test_deploy_portoes_antes_do_migrate.sh:198`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:218`.

- **Censo `PEND`:** escritores `bin/deploy.sh:185`, `bin/deploy.sh:187`; leitores `bin/deploy.sh:189`, `bin/deploy.sh:204`, `bin/deploy.sh:209`, `bin/deploy.sh:212`, `bin/deploy.sh:216`, `bin/deploy.sh:224`. O homônimo em `bin/sondas.sh:18` é caminho de diretório, lido em `bin/sondas.sh:19`, `bin/sondas.sh:21`, `bin/sondas.sh:22`.

- **Censo `APLICAR_MIGRATE`:** escritores `bin/deploy.sh:61`, `bin/deploy.sh:230`; único leitor decisório `bin/deploy.sh:312`. O selo injeta o valor de ambiente em `bin/tests/test_deploy_portoes_antes_do_migrate.sh:109` e cobra sua neutralização em `bin/tests/test_deploy_portoes_antes_do_migrate.sh:170`.

- **Censo `RESTART`:** escritores `bin/deploy.sh:70`, `bin/deploy.sh:73`; leitores `bin/deploy.sh:115`, `bin/deploy.sh:200`, `bin/deploy.sh:219`, `bin/deploy.sh:335`, `bin/deploy.sh:441`. O último escolhe apenas a mensagem final: `bin/deploy.sh:442`, `bin/deploy.sh:444`.

- **Censo `recusa_restart_com_cron`:** definição `bin/deploy.sh:110`; chamadas `bin/deploy.sh:117`, `bin/deploy.sh:338`. O contrato externo reconhece textualmente o ramo `RESTART` e exige chamada antes do restart e definição da função: `bin/contrato_deploy.sh:41`, `bin/contrato_deploy.sh:47`, `bin/contrato_deploy.sh:50`. Esses leitores não exigem migrate no restart nem rechecagem na 1c: `bin/contrato_deploy.sh:41`, `bin/contrato_deploy.sh:45`, `bin/contrato_deploy.sh:47`.

- **Sem leitor decisório com significado antigo identificado:** `PEND` permanece resultado booleano do check; `APLICAR_MIGRATE` continua autorização de apply; `RESTART` continua seleção da publicação por restart. A incompatibilidade encontrada está nas recomendações operacionais recíprocas, quando ambos pendem: `bin/deploy.sh:184`, `bin/deploy.sh:200`, `bin/deploy.sh:225`, `bin/deploy.sh:230`, `bin/deploy.sh:312`, `bin/deploy.sh:335`.

## 09/11 00:38 · sessao 3198ce51949f · - · - · origem=Ronald (tmux)

- **Comentário falso:** `bin/deploy.sh:221` promete recusa “antes de qualquer portao”, porém árvore, cron, auth e sombra já passaram em `bin/deploy.sh:101`, `bin/deploy.sh:117`, `bin/deploy.sh:145` e `bin/deploy.sh:162`. A recusa nova ocorre em `bin/deploy.sh:219`. A promessa de impedir o apply do core é sustentada pela atribuição exclusiva em `bin/deploy.sh:230` e pelo apply em `bin/deploy.sh:312`.

- **Caso 14 ficou com explicação falsa:** o cabeçalho e a mensagem atribuem a recusa à 1c (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:39`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:187`). Mas ele fornece core pendente em `bin/tests/test_deploy_portoes_antes_do_migrate.sh:186`, portanto aborta na seção 1 (`bin/deploy.sh:219`), antes da prova que criaria o cron (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`). A 1c também já não tem recusa por cron (`bin/deploy.sh:312`).

- **Casos 11 e 13 mordem o retorno do migrate no restart:** ambos exigem `RC!=0`, zero apply e zero restart (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:175`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:183`). O contador exclui `--check` e `--plan` (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:111`). No 11, a marca só surgiria no apply falso (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:86`); mesmo que a recusa final por cron evitasse restart, o apply contado reprovaria o caso (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:175`). O 13 também cobre o caminho sem cron injetado (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:182`).

- **Casos 22 e 23 mordem o fechamento indevido do restart:** o 22 injeta somente pendência de mensageria e exige sucesso, zero apply do core e uma chamada restart (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:218`); o 23 exige o mesmo sem pendência injetada (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:222`). Eles cobrem uma recusa ampliada indevidamente para mensageria ou para todo restart (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:219`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:223`).

- **Limites dos quatro casos:** 11/13 não verificam a seção ou mensagem da recusa nem ausência de chamadas aos portões; verificam apenas os três resultados (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:175`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:183`). Mover a recusa para depois dos portões, mantendo-a antes do apply, preservaria essas asserções (`bin/deploy.sh:302`, `bin/deploy.sh:312`). 22/23 contam qualquer linha `restart`, sem conferir os containers (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:120`); o docker falso devolve sucesso sem executar boot (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:94`). Assim, não provam aplicação real da migration de mensageria [nao verificado].

- **Demais comentários novos:** “nunca num --restart”, entendido como apply do core, combina com o fluxo (`bin/deploy.sh:219`, `bin/deploy.sh:230`, `bin/deploy.sh:312`). O boot declarado de mensageria executa migrate (`docker-compose.mensageria.yml:20`). A janela entre a amostragem final de cron e o restart continua explícita no código (`bin/deploy.sh:338`, `bin/deploy.sh:339`); o envelope cria marca sem trava compartilhada e executa o comando uma vez (`bin/cron_run.sh:9`, `bin/cron_run.sh:11`). Testes não executados nesta revisão [nao verificado].

## 09/11 00:38 · sessao 7a58b9f3bf40 · - · - · origem=Ronald (tmux)

1. **Não há caminho ordinário com `RESTART=1` e apply do core.** `APLICAR_MIGRATE` e `RESTART` são zerados, anulando valores herdados; somente o modo `--restart` arma `RESTART`. O único escritor posterior de `APLICAR_MIGRATE=1` está no `else` depois da recusa por `RESTART`; `falhou` encerra o shell. (`bin/deploy.sh:61`, `bin/deploy.sh:70`, `bin/deploy.sh:72`, `bin/deploy.sh:92`, `bin/deploy.sh:219`, `bin/deploy.sh:230`, `bin/deploy.sh:312`)

2. **As opções não são combinadas pelo parser.** Apenas `$1` define o modo; `$2` é motivo em `--restart` e `--sem-sombra`. Portanto, `--restart --sem-migrate` interpreta `--sem-migrate` como motivo, mantendo a recusa do core; na ordem inversa o modo será `--sem-migrate`, sem restart. `DEPLOY_SEM_SOMBRA` altera somente a dispensa da sombra, sem armar migration. (`bin/deploy.sh:56`, `bin/deploy.sh:63`, `bin/deploy.sh:72`, `bin/deploy.sh:74`, `bin/deploy.sh:82`, `bin/deploy.sh:85`, `bin/deploy.sh:86`, `bin/deploy.sh:152`, `bin/deploy.sh:219`)

3. **Mensageria pendente com core sem pendência conserva o restart, condicionado aos outros portões.** `RESTART=1` dispensa a recusa de mensageria; `PEND=0` não arma o apply; o fluxo chega à recusa final por cron e ao comando de restart. O compose declara migration da mensageria no boot. (`bin/deploy.sh:200`, `bin/deploy.sh:204`, `bin/deploy.sh:230`, `bin/deploy.sh:335`, `bin/deploy.sh:338`, `bin/deploy.sh:339`, `docker-compose.mensageria.yml:20`)

4. **Remover a checagem da 1c não deixa caminho sem chamada de recusa antes do restart.** Há recusa inicial e outra imediatamente anterior ao único `docker restart`; `falhou` encerra a execução. Permanece a janela entre a amostragem final e o comando: o envelope cria sua marca independentemente, sem exclusão comum. (`bin/deploy.sh:92`, `bin/deploy.sh:115`, `bin/deploy.sh:117`, `bin/deploy.sh:338`, `bin/deploy.sh:339`, `bin/cron_run.sh:9`, `bin/cron_run.sh:10`, `bin/cron_run.sh:11`)

5. **Comentário falso de ordem:** “Recusa AQUI, antes de qualquer portao” não corresponde ao fluxo. A árvore, o cron e os portões de auth/sombra vêm antes da seção 1; `--sem-sombra` pode inclusive gravar seu log antes dessa recusa. (`bin/deploy.sh:221`, `bin/deploy.sh:101`, `bin/deploy.sh:117`, `bin/deploy.sh:145`, `bin/deploy.sh:160`, `bin/deploy.sh:162`, `bin/deploy.sh:224`)

6. **“Única porta” da migration da mensageria precisa ser lido no escopo do deploy.** O compose executa `migrate` em qualquer boot que use esse comando; o código prova que `--restart` é a porta deste script para provocá-lo. Exclusividade operacional fora dele: [nao verificado]. (`bin/deploy.sh:223`, `bin/deploy.sh:335`, `bin/deploy.sh:339`, `docker-compose.mensageria.yml:20`)

Nenhum teste foi executado; comportamento real dos containers, banco e marcas de produção: [nao verificado]. [nao verificado]

## 09/11 00:35 · sessao 50955c5d0d17 · FIO-E0-V4-E1-FILA · sonda · origem=Code

1. Transcrição dos números fornecidos; verificação independente em produção **[nao verificado]**:

```text
S143 P com destino= 38403
S143 destino de outro colab (vs disputa.colaborador) total= 0 alcancavel_pelo_S143= 0
S143 destino de outro colab (vs P.colaborador) total= 0 alcancavel_pelo_S143= 0
REATAR orfas_abertas= 5525
REATAR orfa com chamado_destino terminal= 1060
REATAR orfa com Chamado terminal do colab no dia (alcancavel pelo HIST/renascer)= 1851
```

2. **A equivalência dos filtros da sonda com o código é [nao verificado]**: foi fornecida a saída, sem o corpo de `sonda_v22_acervo.py`; sua implementação não foi localizada na cópia **[nao verificado]**.

Os filtros efetivos do código são:

- **S143, seleção de perguntas:** destino igual ao chamado, `materializada_em IS NULL` e disputa aberta. Não exige igualdade entre os colaboradores do destino, da disputa e da Pergunta; também não exige `validada_em IS NULL` nem data presente. Depois, pula pergunta em voo, viva, de dia trancado ou respondida sem materialização de fato (`app/chamados/services/disputa_emissao.py:1724`, `1738`, `1744`, `1748`, `1757`). O receiver exige chamado em `ENCERRADOS` e gravação do status (`app/chamados/signals.py:96`, `104`).

- **REATAR, seleção do chamador:** destino **nulo**, colaborador da disputa, disputa aberta, materialização nula e motivo entre os quatro do grupo DIA. Não exige validação nula nem data presente (`app/chamados/reconciliador.py:1496`; `app/chamados/models.py:3690`). Portanto, não se pode equiparar automaticamente esse universo às 5525 “órfãs” da saída: o mesmo dado inclui 1060 “órfãs” com destino terminal **[nao verificado]**.

- **HIST:** só é tentado quando a derivação não encontra destino. Usa `p.data_evento`, com fallback para a data do contexto; busca mesmo colaborador, `ENCERRADOS`, igualdade de `contexto_json.data_turno` e categoria diferente de `disputa_supervisao`. Ainda filtra motivo, desvia a viva para c2 dentro do alcance novo e exige exatamente um candidato histórico (`app/chamados/reconciliador.py:1552`, `1563`, `1582`, `1589`, `1600`, `1603`, `1619`). **HIST não chama o renascimento:** reaponta e chama o resolvedor S128 (`app/chamados/reconciliador.py:1638`, `1644`).

- **c2/reemitir:** chega após HIST vazio e recusa de c1. Aplica gates de motivo, data, colaborador ativo, horizonte, isenção, ausência, protesto, parcial só lista, célula, régua e motor; para intervalos, exige também viva em competência aberta. Abre pela chave colaborador + `batida_ausente` + `data_turno`; só costura se o resultado estiver vivo (`app/chamados/reconciliador.py:1615`, `808`, `820`, `835`, `850`, `855`, `869`, `875`, `886`, `900`, `906`, `908`, `910`, `936`).

A condição isolada **“há terminal do colaborador no dia” é mais larga que a elegibilidade efetiva de HIST/c2**, pelos filtros acima. Se a sonda aplica esses filtros adicionais ou restringe indevidamente validação/data/motivos, é **[nao verificado]**. Também há diferença de vocabulário: `TERMINAIS` inclui cancelado/superado; HIST e o dedup que tenta renascer usam `ENCERRADOS`, apenas resolvido/fechado (`app/chamados/catalogo/motor.py:30`; `app/chamados/reconciliador.py:1583`; `app/chamados/models.py:921`).

3. **Existe caminho de Pergunta até tentativa de renascimento fora de HIST/c2; afirmar que a sonda não o conta é [nao verificado].**

Um caminho concreto é:

- Materialização da Pergunta registra Batida (`app/chamados/services/materializacao.py:926`; `app/ponto/registro_batida.py:157`).
- O signal da Batida chama o cartório (`app/ponto/signals.py:131`, `93`).
- Persistindo furo parcial, com aplicação, sem chamado vivo e sem bloqueio de “parcial só lista”, o cartório chama o emissor (`app/ponto/services/cartorio.py:1767`, `1777`).
- O emissor chama `abrir` por colaborador, módulo e dia, sem filtro de Pergunta nessa chave. Encontrando chamado encerrado, o dedup tenta `_renascer_por_premissa_viva`, sujeito às guardas e à aceitação de `reabrir` (`app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:906`, `921`, `1005`, `1019`).

Esse caminho existe no código; sua ocorrência, quantidade e exclusão dos **1851** informados são **[nao verificado]**.

## 09/11 00:35 · sessao 7bc8cd84a86b · - · - · origem=Ronald (tmux)

Há dois pontos produtivos que chamam `_renascer_por_premissa_viva`: o dedup de `ChamadoColaborador.abrir`, ao encontrar `ENCERRADOS` (`app/chamados/models.py:906`, `app/chamados/models.py:921`), e a resposta tardia no fio, se a célula acusa furo (`app/chamados/services/resposta_tardia.py:35`, `app/chamados/services/resposta_tardia.py:45`).

Caminho concreto a partir de Pergunta, adicional ao HIST/c2:

1. Materialização da resposta hora usa a Pergunta e registra Batida (`app/chamados/services/materializacao.py:794`, `app/chamados/services/materializacao.py:926`).
2. A porta cria Batida (`app/ponto/registro_batida.py:157`); o `post_save` julga os dias (`app/ponto/signals.py:131`, `app/ponto/signals.py:93`).
3. O cartório entrega o emissor a `julgar_colab` (`app/ponto/services/cartorio.py:1964`, `app/ponto/services/cartorio.py:1989`).
4. Persistindo furo parcial, sem chamado vivo, com `apply_` e sem bloqueio `parcial_so_lista`, o cartório chama `_abrir_chamado` (`app/ponto/services/cartorio.py:1767`, `app/ponto/services/cartorio.py:1777`).
5. O emissor chama `ChamadoColaborador.abrir` por colaborador, módulo e `data_turno`, sem filtro de Pergunta nem status nessa chave (`app/ponto/management/commands/detectar_ausencias.py:693`); o dedup pode encontrar um encerrado e tentar renascê-lo (`app/chamados/models.py:906`, `app/chamados/models.py:921`).

A tentativa só efetivamente renasce passando pelas guardas: colaborador ativo, sem encerramento por cascata, competência não trancada, ausência de veto de régua e aceitação por `reabrir` (`app/chamados/models.py:1005`, `app/chamados/models.py:1019`).

A resposta tardia também tenta renascer sem selecionar Pergunta ou órfã; a view busca diretamente o Chamado e chama o serviço (`app/chamados/views.py:1046`, `app/chamados/views.py:1053`). Esse caminho é resposta no fio, não resposta direta de `PerguntaDisputa` (`app/chamados/views.py:1041`).

**Não é possível afirmar que o item (2) da sonda deixa esses caminhos fora da contagem sem a fonte do script** [nao verificado]. O próprio dado fornecido admite “órfã com chamado_destino terminal”; portanto, “órfã” não permite inferir `chamado_destino IS NULL` nesse script [nao verificado].

`_ressuscitar_por_resposta` não é prova de renascimento do status: o corpo apenas levanta o silêncio do destino ou pai com `arquivo.levantar` (`app/chamados/services/materializacao.py:711`, `app/chamados/services/materializacao.py:718`).

## 09/11 00:19 · sessao b599f9f2e362 · FIO-E0-V4-E1-FILA · verificador · origem=Code

A cura produtiva está coerente com a normalização da trava; a cura dos testes ficou **parcial na mensagem e na cobertura da conferência**. A mensagem atual imprime as três parcelas corretamente, mas os testes só exigem os IDs da disputa. (`app/chamados/services/fio.py:2092`; `app/chamados/services/fio.py:2098`; `app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`)

Execução da suíte, conteúdo de `e1_fila_red5.log`, estado do banco e ocorrência em produção: **[nao verificado]**. [nao verificado]

**1. Triagem: o que procede e o que ficou parcial**

| Item da triagem | Verificação estática |
|---|---|
| `None` comparado, mas não travável | **Curado como alinhamento de conjuntos:** `_pks` ignora `None`; `_fora[2]` agora também. Porém, `PerguntaDisputa.disputa` é FK sem `null=True`: o teste fabrica a segunda leitura, sem gravar NULL. Não demonstra uma ocorrência produtiva. (`app/chamados/models.py:2661`; `app/chamados/services/fio.py:2093`; `app/chamados/models.py:2424`; `app/chamados/tests/test_e1_fila.py:509`) |
| Resultado preenchido sem exigir migração | **Curado:** exige `([p.pk], [], [])` e relê o destino gravado. O MORDE sem laço exige lista vazia e destino antigo. (`app/chamados/tests/test_e1_fila.py:547`; `app/chamados/tests/test_e1_fila.py:555`) |
| Diferença trocada por XOR | **Curado para o encolhimento exercitado:** a segunda leitura vazia deve passar. XOR produziria parcelas antigas e recusaria. (`app/chamados/tests/test_e1_fila.py:521`; `app/chamados/services/fio.py:2092`) |
| Cardinalidade no lugar do conjunto de disputas | **Curado:** substitui a disputa mantendo a pergunta e exige recusa. (`app/chamados/tests/test_e1_fila.py:509`; `app/chamados/tests/test_e1_fila.py:513`) |
| Mensagem sem IDs | **Parcial:** exige somente `disputas [1000000000]`; apagar os IDs de colaboradores ou perguntas da mensagem conserva os asserts existentes. (`app/chamados/tests/test_e1_fila.py:494`; `app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`) |
| Duas leituras só conferidas sem exceção | **Curado:** a asserção fica depois do `except`, nos dois ramos. (`app/chamados/tests/test_e1_fila.py:482`) |
| Remoção de `key=str` | **Coerente:** a disputa referencia um `BigAutoField`; `None` sai antes da ordenação. (`app/chamados/migrations/0013_disputasupervisao.py:20`; `app/chamados/services/fio.py:2093`; `app/chamados/services/fio.py:2098`) |

Os dois itens de FILA pertencem à investigação desta obra: são campos usados para escolher as travas, e o escopo inclui seus escritores e leitores. Isso não os transforma em defeitos concorrentes demonstrados; falta um escritor produtivo que altere esses FKs depois do nascimento. (`CLAUDE.md:376`; `app/chamados/services/fio.py:2085`; `app/chamados/models.py:3420`; alcance produtivo **[nao verificado]**)

**2. Quem escreve, quem lê e o que efetivamente se compara**

O conjunto nasce das perguntas apontadas para velho/novo, acrescido de `universo_da_linha` de cada uma. A mesma função é chamada antes e depois da trava. (`app/chamados/services/fio.py:2032`; `app/chamados/services/fio.py:2084`; `app/chamados/services/fio.py:2088`)

| Campo/valor | Escritor | Leitores relevantes ao ato |
|---|---|---|
| `Pergunta.colaborador_id` | No nascimento, `_chave_de_identidade` deriva da disputa ou recusa divergência. Há também o preenchimento histórico da migration 0051. (`app/chamados/models.py:3430`; `app/chamados/migrations/0051_decisao_pergunta.py:25`) | Composição de `_cs`, conferência, trava de Colaborador e juiz da costura. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2092`; `app/chamados/models.py:2674`; `app/chamados/juizes.py:2003`) |
| `Pergunta.pk` | Identidade automática `BigAutoField`; a porta insere com `force_insert=True`. (`app/chamados/migrations/0014_perguntadisputa.py:19`; `app/chamados/models.py:3019`) | Lista para travar Perguntas, conferência e composição dos grupos do laço. (`app/chamados/services/fio.py:2087`; `app/chamados/services/fio.py:2092`; `app/chamados/services/fio.py:2143`) |
| `Pergunta.disputa_id` | A disputa entra no construtor usado pelo nascimento; depois ocorre a inserção. (`app/chamados/models.py:3016`; `app/chamados/models.py:3019`) | Lista para travar Disputas, conferência e portas internas de retratação. (`app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2093`; `app/chamados/services/fio.py:1330`) |
| Donos de velho/novo | O nascimento do Chamado recebe `colaborador`; `criar` o persiste. (`app/chamados/models.py:1054`; `app/chamados/models.py:1095`) | Entram em `_cs` pelas instâncias recebidas; a conferência não relê esses donos. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2092`) |
| Mensagem de `UniversoMudouSobATrava` | Montada com velho/novo e as três parcelas ordenadas. (`app/chamados/services/fio.py:2095`) | `reabrir` captura, registra `str(_e)[:300]` em `depois['erro']` e retorna `False`; o helper de teste captura a string inteira. (`app/chamados/models.py:1417`; `app/chamados/models.py:1424`; `app/chamados/tests/test_e1_fila.py:489`) |

O evento é persistido pela porta `core.observ.evento`, que escreve `descricao` e `valor_depois` em `LogAuditoria`; portanto o registro produtivo pode conter somente os primeiros 300 caracteres do erro. (`app/core/observ.py:94`; `app/core/observ.py:134`; `app/chamados/models.py:1424`)

Se `P0` é a primeira leitura e `P1` a segunda, a relação é esta:

| Classe | IDs pedidos à trava | Parcela que provoca recusa |
|---|---|---|
| Colaborador | `C0 = {dono velho, dono novo} ∪ colaboradores(P0)` | `colaboradores(P1) − C0` |
| Pergunta | `pk(P0)` | `pk(P1) − pk(P0)` |
| Disputa | `disputas(P0) − {None}` após normalização | `(disputas(P1) − disputas(P0)) − {None}` |
| Chamado | `{velho.pk, novo.pk}` | Não há parcela de conferência para Chamado. |

Essas expressões vêm da chamada e da formação de `_fora`; a normalização está em `_pks`. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2092`; `app/chamados/models.py:2661`)

Logo:

- **A conferência exige cobertura, não igualdade:** encolher é permitido; linhas da primeira leitura podem continuar travadas mesmo ausentes na segunda. (`app/chamados/services/fio.py:2092`; `app/chamados/tests/test_e1_fila.py:521`)
- **Para `disputa_id` inteiro, não há filtro divergente entre os lados:** `_pks` não descarta zero ou negativos; descarta `None`, assim como a conferência. (`app/chamados/models.py:2661`; `app/chamados/services/fio.py:2093`)
- **Não há valor `None` comparado como nova disputa faltante na v5.** O inverso possível é uma disputa antiga continuar travada após desaparecer da segunda leitura, comportamento deliberadamente aceito. (`app/chamados/services/fio.py:2093`; `app/chamados/tests/test_e1_fila.py:529`)
- **IDs calculados não são comprovantes de linhas adquiridas:** as queries travam linhas existentes, e a migração não usa o retorno da trava para conferir aquisições. Integridade efetiva do banco: **[nao verificado]**. (`app/chamados/models.py:2674`; `app/chamados/models.py:2683`; `app/chamados/models.py:2702`; `app/chamados/services/fio.py:2086`)

A ordem implementada é Colaborador → Disputa → Chamado → Pergunta, por PK em cada classe. (`app/chamados/models.py:2674`; `app/chamados/models.py:2683`; `app/chamados/models.py:2696`; `app/chamados/models.py:2700`)

**3. Mordida dos testes e mutantes residuais** [nao verificado]

Os testes novos distinguem estaticamente retirar `-{None}`, usar XOR, comparar cardinalidade das disputas e suprimir o laço. O instrumento de mutação recompila a função real e exige uma única ocorrência da âncora. (`app/chamados/tests/test_e1_fila.py:48`; `app/chamados/tests/test_e1_fila.py:513`; `app/chamados/tests/test_e1_fila.py:521`; `app/chamados/tests/test_e1_fila.py:526`; `app/chamados/tests/test_e1_fila.py:555`)

Persistem estes mutantes plausíveis **pelos oráculos inspecionados**; sobrevivência executada na suíte inteira: **[nao verificado]**. [nao verificado]

| Mutante | Por que os asserts atuais não o distinguem |
|---|---|
| Na mensagem, trocar `sorted(_fora[0])` ou `sorted(_fora[1])` por `[]` | Colaborador/Pergunta exigem apenas mensagem verdadeira; só Disputa tem conteúdo exigido. (`app/chamados/services/fio.py:2098`; `app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`) |
| Imprimir todas as disputas de `_sob`, em vez de somente as externas | No caso que verifica texto, a única disputa foi substituída; o caso que acrescenta disputa só exige recusa. (`app/chamados/tests/test_e1_fila.py:502`; `app/chamados/tests/test_e1_fila.py:509`; `app/chamados/tests/test_e1_fila.py:519`) |
| Trocar a condição `_fora[1]` por crescimento da cardinalidade dos PKs | Há acréscimo de PK e esvaziamento, mas não substituição de PK mantendo cardinalidade. (`app/chamados/services/fio.py:2094`; `app/chamados/tests/test_e1_fila.py:498`; `app/chamados/tests/test_e1_fila.py:523`) |
| Comparar apenas cardinalidades dos colaboradores de `P1` e `P0` | O caso exclusivo de colaborador acrescenta um valor; não substitui o conjunto mantendo tamanho. (`app/chamados/tests/test_e1_fila.py:500`; `app/chamados/tests/test_e1_fila.py:509`) |
| Executar o laço imediatamente antes do `if _fora` | Os casos de corrida usam costura cruzada, já recusada pela porta; depois a exceção continua ocorrendo. O cenário efetivamente migrável só é usado sem corrida. (`app/chamados/tests/test_e1_fila.py:147`; `app/chamados/tests/test_e1_fila.py:547`; `app/chamados/tests/test_e1_fila.py:563`; `app/chamados/services/fio.py:863`) |

O teste “nada migra” comprova recusa e destino final nesse cenário; **não comprova que uma migração possível foi impedida ou desfeita**. A segunda leitura é simulada, enquanto o laço volta a consultar o banco real. (`app/chamados/tests/test_e1_fila.py:474`; `app/chamados/tests/test_e1_fila.py:563`; `app/chamados/services/fio.py:2118`; `app/chamados/services/fio.py:2122`)

**4. Escritores produtivos dos dois itens de FILA**

**Não identifiquei escritor produtivo que troque esses FKs após o nascimento; não há prova universal de ausência — alcance concorrente [nao verificado].**

- **Dono do Chamado:** sua fotografia realmente não é relida para `_cs`. A atribuição aparente em `pergunta_no_app` apenas compartilha a instância do mesmo colaborador dentro de grupos formados pelo próprio ID. A porta dinâmica de texto aceita somente título, assunto e descrição. (`app/chamados/services/fio.py:2085`; `app/chamados/services/pergunta_no_app.py:133`; `app/chamados/services/pergunta_no_app.py:139`; `app/chamados/services/texto.py:18`; `app/chamados/services/texto.py:25`)
- **Colaborador/disputa da Pergunta:** a possibilidade programática existe: depois de `_state.adding`, `save` não repete a guarda da identidade. Isso prova permissividade da API, não um escritor que efetivamente altere os campos. (`app/chamados/models.py:3420`; `app/chamados/models.py:3423`; `app/chamados/models.py:3430`)
- **Escritor histórico encontrado:** a migration 0051 preenche `colaborador_id` por `Subquery`, via `RunPython`; não é um caminho da execução normal da migração de resolvedoras. (`app/chamados/migrations/0051_decisao_pergunta.py:25`; `app/chamados/migrations/0051_decisao_pergunta.py:101`)
- **Porta ampla encontrada:** `descarimbar_em_lote(**outros)` admite outros campos, mas os chamadores rastreados passam resposta/validação. O outro lote também usa dicionário fixo desses campos. (`app/chamados/models.py:3252`; `app/chamados/models.py:3266`; `app/chamados/models.py:1565`; `app/chamados/services/disputa_emissao.py:1517`; `app/chamados/services/disputa_emissao.py:1554`; `app/chamados/services/disputa_emissao.py:1556`)
- **Escritas deste ato:** reapontamento grava destino/lavra, eventualmente dia; retratação grava carimbo e campos associados. Não alteram os três FKs investigados. (`app/chamados/services/fio.py:866`; `app/chamados/services/fio.py:879`; `app/chamados/services/fio.py:922`; `app/chamados/models.py:3128`; `app/chamados/models.py:3152`)

Portanto, os dois FILA sustentam **riscos latentes da interface**, mas não sustentam, com a evidência disponível, uma corrida produtiva bloqueante. (`app/chamados/models.py:3420`; `app/chamados/services/fio.py:2085`; corrida produtiva **[nao verificado]**)

**5. Classes que já reprovaram obra**

| Classe | Resultado nesta verificação |
|---|---|
| Segundo juiz duplicando registrado | **Não nas linhas tocadas:** a guarda decide cobertura de travas; vitalidade continua delegada a `pergunta_viva`, registrada como autoridade. (`app/chamados/services/fio.py:2026`; `app/chamados/services/fio.py:2113`; `app/core/juizes.py:266`) |
| Segundo escritor fora da porta | **Não nas linhas tocadas:** o diff modifica conjuntos/mensagem; o laço continua usando reapontamento e retratação pelas portas existentes. (`app/chamados/services/fio.py:2092`; `app/chamados/services/fio.py:2165`; `app/chamados/services/fio.py:2171`) |
| Relógio fora da borda | **Não no delta v5:** a conferência não lê relógio. No caminho histórico de expiração, `_hoje` usa `timezone.now` ou o instante recebido. (`app/chamados/services/fio.py:2092`; `app/chamados/veredito_lavrado.py:95`) |
| Derivação paralela de fonte única | **Não na construção do universo:** as duas leituras usam `_conjunto_da_migracao`; esta e o laço usam `universo_da_linha`. (`app/chamados/services/fio.py:2032`; `app/chamados/services/fio.py:2084`; `app/chamados/services/fio.py:2088`; `app/chamados/services/fio.py:2142`) |
| Selo/RED sem mordida | **Sim, residual de cobertura:** IDs de duas classes, substituição de PK e precedência da conferência ainda têm os escapes descritos acima. (`app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`; `app/chamados/tests/test_e1_fila.py:563`) |
| Ordem de trava | **Não há inversão demonstrada neste ato:** uma chamada adquire as quatro classes na ordem; crescimento provoca exceção antes do laço. (`app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2094`; `app/chamados/models.py:2674`; `app/chamados/models.py:2700`) |
| Pouso antes do pré-requisito | **[nao verificado]**. A regra exige suíte verde antes do opus e opus antes do pouso; os anexos não comprovam essa sequência. (`CLAUDE.md:374`; `CLAUDE.md:378`) | [nao verificado]
| Campo com dois significados | **Não introduzido pelo delta:** `disputa_id` continua sendo FK de Disputa; `None` é ignorado na normalização. A igualdade colaborador da Pergunta/disputa é verificada apenas no nascimento; divergência produtiva posterior **[nao verificado]**. (`app/chamados/models.py:2424`; `app/chamados/models.py:2661`; `app/chamados/models.py:3420`; `app/chamados/models.py:3433`) |

## 09/11 00:17 · sessao aad97d481b14 · - · - · origem=Ronald (tmux)

Não localizei escritor de produção que **mude**, após o nascimento, `ChamadoColaborador.colaborador_id`, `PerguntaDisputa.colaborador_id` ou `PerguntaDisputa.disputa_id`. O alcance concorrente dos dois itens de FILA permanece **[nao verificado]**, sem escritor concreto demonstrado.

Evidências e exclusões:

- **Nascimento da Pergunta:** `disputa` entra no construtor e a inserção ocorre com `force_insert=True` (`app/chamados/models.py:3016`, `app/chamados/models.py:3019`); `colaborador_id` é derivado da disputa ou validado contra ela (`app/chamados/models.py:3430`, `app/chamados/models.py:3432`, `app/chamados/models.py:3433`).
- **Mutabilidade latente existe:** `save()` só chama a guarda da identidade quando `_state.adding` (`app/chamados/models.py:3420`); depois repassa ao `super().save()` (`app/chamados/models.py:3423`). Isso permite escrita programática futura, mas não demonstra um chamador produtivo que a faça.
- **Escritor histórico:** a migração preenche `colaborador_id` por `Subquery` (`app/chamados/migrations/0051_decisao_pergunta.py:25`), registrada em `RunPython` (`app/chamados/migrations/0051_decisao_pergunta.py:101`). Não é escritor da execução normal de `migrar_resolvedoras_vivas`.
- **A atribuição aparente `colaborador_id` no fluxo de abertura é proxy:** o objeto é `_Proxy`, não modelo persistido (`app/chamados/models.py:2888`, `app/chamados/models.py:2890`, `app/chamados/models.py:2894`).
- **A atribuição aparente de dono do Chamado não troca o ID:** os chamados são agrupados pelo próprio `colaborador_id`, e a mesma instância é compartilhada no grupo, sem save (`app/chamados/services/pergunta_no_app.py:133`, `app/chamados/services/pergunta_no_app.py:139`, `app/chamados/services/pergunta_no_app.py:141`).
- **`descarimbar_em_lote(**outros)` seria uma porta ampla:** aceita outros campos e escreve por `update` (`app/chamados/models.py:3252`, `app/chamados/models.py:3266`), mas os chamadores encontrados passam apenas resposta/validação (`app/chamados/models.py:1565`; `app/chamados/services/disputa_emissao.py:1517`, `app/chamados/services/disputa_emissao.py:1554`).
- **Outros lotes:** `reabrir_para_correcao` escreve o dicionário fixo de resposta/validação (`app/chamados/services/disputa_emissao.py:1517`, `app/chamados/services/disputa_emissao.py:1556`); `bulk_update` escreve somente a lavra (`app/chamados/veredito_lavrado.py:217`, `app/chamados/veredito_lavrado.py:230`).
- **Migração:** o reapontamento salva destino/lavra, eventualmente o dia (`app/chamados/services/fio.py:866`, `app/chamados/services/fio.py:879`, `app/chamados/services/fio.py:922`, `app/chamados/services/fio.py:924`); a retratação salva carimbo, resposta/validação e lavra (`app/chamados/models.py:3128`, `app/chamados/models.py:3133`, `app/chamados/models.py:3137`, `app/chamados/models.py:3151`, `app/chamados/models.py:3152`).
- **`setattr` do texto do Chamado:** restrito a `titulo`, `assunto`, `descricao`, com recusa dos demais campos (`app/chamados/services/texto.py:18`, `app/chamados/services/texto.py:25`, `app/chamados/services/texto.py:27`, `app/chamados/services/texto.py:33`).
- **`setattr` genérico de reexibição:** só altera memória, sem salvar (`app/core/reexibir.py:21`, `app/core/reexibir.py:45`, `app/core/reexibir.py:50`); seus chamadores são de colaboradores (`app/colaboradores/views.py:1094`, `app/colaboradores/views.py:1259`).
- **Admin:** `app/chamados/admin.py` está vazio; a ausência também é documentada (`app/core/contratos_estruturais.py:331`). Não encontrei registro desses modelos nos outros admins nem ModelForm/ModelSerializer que os edite **[nao verificado: ausência universal]**. O `ready()` do app só importa signals (`app/chamados/apps.py:8`, `app/chamados/apps.py:10`).

Rodei o censo AST puro em memória, sem Django nem gravação de arquivo. Ele confirma os sítios acima, mas não fornece prova formal de ausência: o próprio censo admite operações não atribuíveis e inferências (`app/core/censo_escritas.py:38`, `app/core/censo_escritas.py:43`, `app/core/censo_escritas.py:49`).

_2962 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
