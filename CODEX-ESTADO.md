# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 21:41** · publicado em **06/11/2026 21:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 21:41 · sessao 87b166d0bc57 · - · - · origem=Ronald (tmux)

(1) **Universo: não há predicado seguro por motivo + data isoladamente.** O recorte migrável precisa demonstrar a fonte antiga e comparar `p.data_evento = dia da saída da fonte` com `dia_do_turno_da_batida(colab, batida_fonte) != p.data_evento`; o juiz faz o recorte por PK da batida sobre `dia_das_batidas`, sem o backfill criar uma regra “subtrair um dia”. (`app/ponto/turnos.py:2917`, `app/ponto/turnos.py:2923`, `app/ponto/turnos.py:2939`)

- Os motivos diretamente envolvidos são `orfao_14h` (falta S) e `saida_sem_entrada` (falta E). A implementação atual documenta que antes o primeiro recebia a data da saída esperada e o segundo a data da saída órfã. Isso documenta a história na raia, mas não substitui abertura do main `19aeede1`, aqui **[nao verificado]**. (`app/escala/servico_jornada.py:474`, `app/escala/servico_jornada.py:480`, `app/escala/servico_jornada.py:485`)
- Fontes atuais: detector encontra a entrada pendente pelo PK e usa `resolver_data_evento`; questionário de chamados usa a batida origem; API e clone corrigem o contexto depois de registrar a S; triagem usa o mesmo juiz da batida. (`app/chamados/detector_anomalias.py:39`, `app/chamados/detector_anomalias.py:47`, `app/chamados/services/disputa_emissao.py:1068`, `app/chamados/services/disputa_emissao.py:1080`, `app/api/views.py:775`, `app/api/views_core.py:775`, `app/ponto/services/triagem_batida.py:726`)
- Outros caminhos da raia que passaram a obter o dia da batida: referência da grade por `entrada_id`, instante exato ou `ultima_batida.id`; lote de entradas pendentes por PK; fallback da última batida por ID. (`app/chamados/services/disputa_emissao.py:59`, `app/chamados/services/disputa_emissao.py:68`, `app/chamados/services/disputa_emissao.py:272`, `app/chamados/services/disputa_emissao.py:282`, `app/chamados/services/disputa_emissao.py:379`)
- `_T1` concreto é `_data_do_turno`, chamado por `_mk`. Para S órfã ele consulta o envelope da jornada da véspera; sem marcos da véspera, retorna o dia civil da referência. Portanto “sem marcos” não autoriza o backfill a inventar D−1: origem histórica suficiente ou conclusão **[nao verificado]**. (`app/ponto/turnos.py:248`, `app/ponto/turnos.py:285`, `app/ponto/turnos.py:293`, `app/ponto/turnos.py:312`)
- A casca `turnos_do_colab` carrega células da janela, escalas incluindo a geradora da célula e marcos do DNA. `dia_do_turno_da_batida` usa essa autoridade por `dia_das_batidas`; batida não reclamada por turno recebe o dia civil. (`app/ponto/turnos.py:1850`, `app/ponto/turnos.py:1884`, `app/ponto/turnos.py:1909`, `app/ponto/turnos.py:2902`, `app/ponto/turnos.py:2944`)
- Contraexemplos de código ao filtro por motivo: perguntas de declaração de marco da célula já recebem `dia_do_turno_da_celula(dia)`; uma pergunta `orfao_14h` ou `saida_sem_entrada` pode, portanto, já conter o significado correto. Turno diurno não atravessa a meia-noite no juiz e conserva o dia próprio. Exemplares reais nos anexos: **[nao verificado]**. (`app/chamados/services/declaracao_texto.py:325`, `app/chamados/services/declaracao_texto.py:615`, `app/ponto/turnos.py:289`, `app/ponto/turnos.py:2929`)

(4) **Censo fechado da raia, executado sem Django/DB, usando o próprio varredor AST extraído:** quatro sítios, sendo apenas dois persistentes: `_nascer` cria a pergunta; `reapontar_resolvedora` preenche a data ausente. Os outros são proxy de validação e reversão em memória. O algoritmo e as duas portas sancionadas estão no selo. (`app/chamados/models.py:2444`, `app/chamados/models.py:2498`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:846`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:28`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:112`)

**Nenhuma porta atual re-chaveia uma pergunta com data preenchida preservando sua identidade.** `fio.reapontar_resolvedora` só escreve quando `not p.data_evento`; `abrir` consulta o fato no dia pedido e devolve outra pergunta existente ou cria nova. Logo o command deve ser gatilho de uma extensão da porta canônica, com trilha, e não atribuir diretamente o campo; a extensão ainda não existe nesta cópia. (`app/chamados/services/fio.py:835`, `app/chamados/models.py:2453`, `app/chamados/models.py:2497`, `app/chamados/models.py:2498`, `app/chamados/services/fio.py:863`)

O contrato antigo admite somente os arquivos `models.py` e `fio.py` para `data_evento`; escritor novo fora deles falha ou exige declaração justificada. A raia acrescenta selo AST mais restrito: somente `_nascer` e `reapontar_resolvedora`, incluindo `bulk_update`, `setattr`, kwargs e aliases do modelo. Um command de backfill direto viola o desenho e o selo atuais. (`app/core/tests/test_contract_escritores_estado.py:48`, `app/core/tests/test_contract_escritores_estado.py:62`, `app/core/tests/test_contract_escritores_estado.py:200`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:13`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:133`)

A data precisa entrar antes do juiz e da lavra; o save da porta inclui `veredito_lavrado`, `julgado_em` e `janela_expira_em`. Portanto uma extensão de re-chaveamento não deve simplesmente salvar `data_evento` isoladamente. (`app/chamados/services/fio.py:819`, `app/chamados/services/fio.py:848`)

**Colisão:** identidade é `(colaborador, motivo, data_evento)` em qualquer disputa/estado; `abrir` pega a última por PK e retorna a existente, sem transferir a pergunta antiga, respostas ou lastro. (`app/chamados/models.py:2385`, `app/chamados/models.py:2396`, `app/chamados/models.py:2403`, `app/chamados/models.py:2453`, `app/chamados/models.py:2497`)

A lei já existente para duplicatas escolhe quem tem resposta, depois menor PK; a outra recebe `VIA_DUPLICATA` e motivo com a canônica, sem apagar e mantendo carimbo anterior. A migração de resolvedoras aplica essa lei às vivas que conflitam no destino. Uma fusão automática de duas materializadas no re-chaveamento não está implementada por essas portas **[nao verificado quanto a outra porta não aberta]**. (`app/chamados/services/fio.py:870`, `app/chamados/services/fio.py:881`, `app/chamados/services/fio.py:894`, `app/chamados/services/fio.py:934`, `app/chamados/services/fio.py:940`)

## 06/11 21:40 · sessao 579cf276cb5a · - · - · origem=Ronald (tmux)

**(2) O lastro que a migração precisa conservar**

- Para pergunta `hora`, `_slot_esperado` chama o materializador real; o reconhecedor busca `Batida.pergunta_origem` não retratada e compara janela de ±20 minutos nos dias `data_evento+0/+1`. Portanto, um plantio em D−1 só passa a caber no offset 0 após a pergunta receber o D do turno; ter FK de origem, isoladamente, não prova a materialização. (`app/chamados/juizes.py:245`, `app/chamados/juizes.py:262`, `app/chamados/juizes.py:271`, `app/chamados/juizes.py:344`, `app/chamados/juizes.py:719`)
- O vínculo da batida é FK para o PK da pergunta; conservar PK mantém esse vínculo sem mudar timestamp nem fato. O materializador grava a origem tanto na batida nova como na reutilizada. (`app/ponto/models.py:40`, `app/chamados/services/materializacao.py:917`)
- Migrar apenas `data_evento` pode deixar a FK `p.celula` no outro dia: `_celulas_da_pergunta` só aceita a célula quando data e colaborador coincidem; o juiz de vitalidade e o juiz do marco leem diretamente a célula da pergunta. Deve-se reatar à **célula existente do dia correto**, sem deslocar `CelulaDia.data` ou seu DNA. (`app/chamados/juizes.py:622`, `app/chamados/juizes.py:1148`, `app/chamados/juizes.py:1182`, `app/chamados/juizes.py:890`, `app/escala/models.py:1685`)
- O chamado que lista a pergunta também precisa continuar falando desse mesmo fato: `data_do_fato_de` compara `data_evento` com `dia_do_fato(ch, fortes=True)`; célula do chamado vence o contexto; divergência sem conferência bloqueia a materialização. Revisar `chamado_destino`, sua célula e sua chave de data, não somente o JSON. (`app/chamados/services/fio.py:77`, `app/chamados/juizes.py:94`, `app/chamados/juizes.py:107`, `app/chamados/services/materializacao.py:1167`)
- `DisputaSupervisao` não tem coluna própria de dia: possui `contexto`, chamado e perguntas; a derivação fraca do dia do chamado pode usar o mínimo das datas das perguntas da disputa. O contexto por motivo também alimenta `_turno_tem_saida` quando a pergunta não tem data. (`app/chamados/models.py:1679`, `app/chamados/models.py:1683`, `app/chamados/juizes.py:56`, `app/chamados/juizes.py:363`)
- Não se deve reescrever historicamente `EventoCelula` como se fosse a pergunta: ele registra a lavra da célula, com `celula_id`, colaborador e data, sem FK. A porta emite um **novo evento** da célula correta. O `post_save` da pergunta acorda o consumidor quando mudam `data_evento`, célula, disputa ou destino; ele usa a célula da FK antes do fallback pelo par colaborador/data, portanto FK errada emite evento do dia errado. (`app/escala/models.py:1845`, `app/ponto/portas/celula.py:882`, `app/chamados/models.py:2682`, `app/chamados/models.py:2693`, `app/chamados/signals.py:214`)
- O consumidor relê células e chamados atuais e pode rejuntar, carimbar perguntas e retratar chamados; logo esse evento é efeito operacional da migração que precisa constar da sombra. (`app/chamados/reconciliador.py:1305`, `app/chamados/reconciliador.py:1346`)

**Absorção exige preservar o grupo, não só a pergunta.** O escritor carimba todas as irmãs `hora` daquele dia com o mesmo instante; a recuperação usa `(disputa, data_evento, materializada_em)`, exige escape `FALTA_DIA/ATESTADO`, casa com `(colaborador, tipo, Ausencia.data)` e exclui perguntas com batida plantada viva. Rechavear isoladamente uma irmã rompe essa associação. Para conservar exatamente a ligação atual, dia do grupo completo e dia da ausência têm de continuar coerentes, preservando disputa/instante; isso **não autoriza** mudar `Ausencia.data` sem provar que seu próprio fato pertence ao dia do turno. Se a ausência pertence corretamente ao dia civil e a pergunta ao dia do turno, o desenho atual não expressa essa distinção. (`app/chamados/services/materializacao.py:530`, `app/chamados/services/materializacao.py:586`, `app/chamados/services/materializacao.py:606`)

Quais perguntas do dado são absorvidas, quais têm FK de origem e quais plantios estão em D−1: **[nao verificado]**.

**(3) Condição exata e “dispararia 0”**

A função relê cada PK sob trava; a instância recebida já modificada em memória não é usada como estado pós-migração. Só dispara quando:

1. a linha gravada ainda satisfaz `materializada_em IS NOT NULL`;
2. a via está fora de `VIAS_DE_IDENTIDADE`;
3. com `reabrir_fechada=False`, a disputa permanece aberta;
4. `pergunta_viva(p)` retorna `True`. (`app/chamados/services/relavra_via.py:169`, `app/chamados/services/relavra_via.py:217`)

As vias de identidade são `admin_decidiu`, `duplicata`, `fato_ocupado`, `paridade_recusada`. (`app/chamados/models.py:2290`)

Ao disparar, `descarimbar()` zera conjuntamente `materializada_em`, `materializada_por`, `via_resolucao`, emite trilha e, no comportamento padrão, reabre disputa fechada. (`app/chamados/models.py:2668`, `app/chamados/services/relavra_via.py:229`)

**Perder reconhecimento do plantio não implica automaticamente descarimbo.** `validada_em` preenchida mata a pergunta logo no primeiro degrau; tranca também a mata; resposta não vazia, célula acusando e lei do dia podem decidir antes do último degrau de reconhecimento. O último degrau devolve o inverso de `_pergunta_materializada_de_fato`; exceção nele retorna `False`, portanto “zero” com juiz indisponível seria uma falsa certificação. (`app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1151`, `app/chamados/juizes.py:1178`, `app/chamados/juizes.py:1198`, `app/chamados/juizes.py:1208`)

O gancho atual do julgamento coleta elegíveis pela mesma expressão `q_carimbo_desfazivel(so_disputa_aberta=True)` e chama a função real com `reabrir_fechada=False`. (`app/chamados/services/fabrica_da_celula.py:117`, `app/chamados/services/fabrica_da_celula.py:163`)

Para uma sombra **sem escrever**, a função não possui `dry_run`: seria necessário executar a função real sobre uma projeção pós-migração que sua releitura ORM enxergue, mantendo os juízes reais e substituindo apenas os efeitos `descarimbar`, `evento` e `reabrir_sistema` por espiões; contar o retorno real e exigir retorno 0, nenhum efeito chamado e nenhuma exceção/engolido. Proposta de instrumentação, ainda não executada: **[nao verificado]**. Não basta passar cópias com datas trocadas: a releitura por PK ignora essas cópias. (`app/chamados/services/relavra_via.py:213`)

O precedente documentado chama a função real em `atomic+raise`, mas isso executa escritas e depois as desfaz; não satisfaz “sem escrever”. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:23`)

O contador existente `materializada_viva()` compartilha elegibilidade e juiz, porém separa `aberta` de `exportada` e só inclui disputa aberta; `aberta=0` sozinho não prova que a função padrão dispararia zero para todo o lote. (`app/chamados/services/vigia_fio.py:789`, `app/chamados/services/vigia_fio.py:806`, `app/chamados/services/vigia_fio.py:847`)

## 06/11 21:40 · sessao 0f08dec0b063 · FIO-COM-ASSENTO-E0-V4 · leitura · origem=Code

**A v4 elimina o escritor de estado derivado da tarefa, mas suas entradas ainda não existem completas na cópia:** decisões são sobrescritas/apagadas, alguns efeitos precisam de consulta externa à célula e a ligação de absorção depende de `materializada_em`. Isso exige completar a alimentação; não demonstra necessidade de uma entidade de obrigação. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/models.py:1976`; `app/chamados/juizes.py:720`; `app/chamados/services/materializacao.py:588`.

Vínculo desta cópia ao HASH/HEAD, execução de testes, custos medidos e ocorrências em produção: **[nao verificado]**. Os exemplos abaixo são construções para examinar informação disponível, não relatos de produção. **[nao verificado]** [nao verificado]

**(1) Entradas existentes e lacunas**

O lookup distingue autoridades por domínio e remete ao registro vivo; ele não registra uma função que reúna toda a pergunta da v4. `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:20`; `app/core/juizes.py:175`.

| Entrada | Fonte existente e retorno | Universo e unidade |
|---|---|---|
| Célula, DNA e ata atuais | `celulas_do_periodo(ids, ini, fim)` → mapa `{(colaborador_id, data): CelulaDia}`; contém a foto persistida, não uma sequência histórica. `app/escala/alimentacao.py:54` | Colaboradores explicitados, faixa inclusiva, sem exigir situação ativa; chave por data da célula. `app/escala/alimentacao.py:65` |
| Fatos concorrentes e precedência | `fatos_do_dia`/`fatos_do_periodo` → tipo vencedor, faixa, rótulo, suprimidos, previsto e fatos encontrados. Não substituem o conteúdo inteiro da célula. `app/ponto/precedencia.py:242` | Pessoa e datas consultadas; ausências, feriados e folgas usam vigência por data. `app/ponto/precedencia.py:259`; `app/ponto/precedencia.py:273` |
| Batidas consideradas na apuração | `batidas_apuraveis` → queryset sem retratadas; batida pendente de DP continua entrando. `app/ponto/turnos.py:2844` | Pessoa opcional; intervalo inclusivo de **instantes**, ou vida inteira sem limites. Não é recorte por dia do turno. `app/ponto/turnos.py:2852`; `app/ponto/turnos.py:2864` |
| Pertencimento ao turno | `turnos_do_colab`; `batidas_do_dia_turno` → turnos ou batidas cuja jornada pertence ao dia pedido. `app/core/juizes.py:196`; `app/ponto/turnos.py:3013` | Dia do **turno**, inclusive batidas civis em D+1. `app/ponto/turnos.py:3017` |
| Marcos previstos | `EscalaColaborador.marcos_do_dia`; `marcos_turno` → marcos e seus datetimes encadeados. `app/core/juizes.py:201`; `app/escala/servico_jornada.py:61` | Dia do turno; o encadeamento avança a data quando atravessa meia-noite. `app/escala/servico_jornada.py:75` |
| Teto temporal | `marcos_vencidos(c, cel, data, agora)` → `(vencidos, pendentes)`, listas `(tipo, HH:MM)`, comparando instante do marco + teto. `app/ponto/services/cartorio.py:831` | Instante aware, ancorado no turno; teto atual de 30 minutos. `app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:845` |
| Turno encerrado | `dia_encerrado`/`dias_encerrados`; `CargaDaJanela.dias_encerrados` reutiliza o corpo existente. `app/core/juizes.py:223`; `app/ponto/turnos.py:3001` | Fato encerrado do turno, não simplesmente `data < hoje`. `app/core/juizes.py:210` |
| Respostas | Campos atuais de `PerguntaDisputa`; `RespostaAnterior` guarda versões retiradas pela reabertura. `app/chamados/models.py:2227`; `app/chamados/models.py:3165` | Resposta ligada à pergunta; `data_evento` é data, resposta/validação são instantes. A versão anterior não constitui histórico completo de todas as alterações. `app/chamados/models.py:2218`; `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1406` |
| Validações e devoluções | `_carimbar_validacao` grava instante/autor; `devolver_validacao_recusada` limpa ambos. São comandos escritores, não um feed imutável. `app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:188` | Pergunta existente, por instante. O estado atual perde a validação desfeita se a alimentação não incluir sua trilha. `app/chamados/services/validacao.py:176` |
| Decisões sobre ausência | Linha atual de `Ausencia`; cobertura por `ausencia_cobre`/`cobertura_ausencia_periodo`; parcialidade por funções próprias. `app/ponto/turnos.py:2536`; `app/ponto/turnos.py:2733`; `app/ponto/turnos.py:2583` | Cobrança exclui rejeitadas; folha exige aprovadas; cobertura por datas não determina sozinha cobertura de cada marco. `app/ponto/turnos.py:2509`; `app/ponto/turnos.py:2542`; `app/ponto/turnos.py:2668` |
| Atos administrativos | Veredito/via da célula; fechamento/reabertura da disputa; conferência de data; silêncio; recusa. Fontes persistidas distribuídas. `app/ponto/portas/celula.py:811`; `app/chamados/models.py:1861`; `app/chamados/services/validacao.py:617`; `app/chamados/services/arquivo.py:166`; `app/chamados/services/validacao.py:545` | Alguns atos atingem o dia, outros pergunta, disputa ou chamado; seus campos atuais não são um registro imutável uniforme. `app/chamados/juizes.py:1148`; `app/chamados/models.py:2063` |
| Reescrita do previsto | `carimbos_de_reescrita` → par regeneração/edição por data; `cobranca_orfa` compara com nascimento da cobrança. `app/escala/services/regeneracao.py:87`; `app/escala/services/regeneracao.py:141` | Data da célula e instantes de reescrita/cobrança; fallback histórico usa `LogAuditoria`. `app/escala/services/regeneracao.py:37`; `app/escala/services/regeneracao.py:108` |
| Responsável e porta existentes | `porta_alcancavel_do_dia` → `None` ou `{rota, fila, verbos, fio}`; admite várias portas/verbos. `app/chamados/juizes.py:1341` | Célula, universo da fila e perguntas no app; exclui acesso manual e “Resolver dia”. Não identifica uma pessoa nominal responsável. `app/chamados/juizes.py:1350`; `app/chamados/juizes.py:1367` |

Há ainda entradas que não cabem apenas em “fatos da célula”: situação do colaborador, veto de cadastro/régua, tranca e contexto de acesso. A fábrica exige ativo; o veto responde separadamente; permissão depende do usuário e do departamento do chamado. `app/chamados/services/fabrica_da_celula.py:91`; `app/ponto/services/furos_vetados.py:27`; `app/chamados/juizes.py:1140`; `app/colaboradores/mixins.py:322`; `app/chamados/utils.py:68`.

**Entrada sem fonte completa hoje:** sequência preservada e ordenada de decisões, contendo correções, revogações, reaberturas e vínculos aos efeitos/marcos que cada decisão atingiu. A resposta ordinária sobrescreve; a reabertura preserva somente parte dos campos; a remoção de motivo apaga a pergunta e suas versões por cascata. Existência de outro feed completo fora das fontes examinadas: **[nao verificado]**. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1407`; `app/chamados/models.py:1976`; `app/chamados/models.py:3179`.

A fábrica também não supre essa lacuna: `motivos_a_perguntar` responde “o que ainda não foi perguntado”, excluindo motivos encontrados no histórico, e não “qual tarefa permanece exigível”. `app/chamados/services/fabrica_da_celula.py:74`; `app/chamados/services/fabrica_da_celula.py:118`.

**(2) Derivabilidade: separar entradas completas de snapshots atuais**

Não encontrei contraexemplo demonstrando que **entradas completas, ordenadas e imutáveis**, com os efeitos e vínculos necessários, exigiriam duas saídas diferentes pela regra vigente. Portanto, não há prova aqui de que os quatro conceitos necessitem de tabela própria de tarefa. Essa impossibilidade geral permanece **[nao verificado]**; as perdas efetivas dos snapshots estão nos escritores abaixo. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/models.py:1976`.

| Conceito | O que pode sair das entradas; onde falta informação |
|---|---|
| **Episódio** | Pode ser reduzido de uma história completa, desde que esteja definido qual acontecimento inicia outro episódio. A identidade atual usa colaborador/motivo/data, sem episódio. O significado universal de episódio pós-reabertura não está implementado nesses caminhos. `app/chamados/models.py:2392`; `app/chamados/models.py:2398` |
| **Pós-condição** | É derivável dos efeitos externos alimentados. Não sai de validação + célula apenas. C6 mata pela validação antes de exigir testemunha, enquanto o plantio de hora faz verificação adicional. `app/chamados/juizes.py:1135`; `app/chamados/services/materializacao.py:1207` |
| **Marco** | Os marcos faltantes podem ser enumerados; falta identidade suficiente para atribuir universalmente uma decisão a uma **instância** quando há repetição. O próprio juiz retorna desconhecido com duas pausas. `app/chamados/services/declaracao_texto.py:65`; `app/chamados/juizes.py:1107` |
| **Equivalência de resposta** | Igualdade do efeito pretendido pode ser calculada com tipo, marco, data e contexto completos. Não encontrei juiz geral de equivalência resposta×resposta: o existente compara resposta×luz e classifica delta. `app/chamados/juizes.py:1087`; `app/chamados/juizes.py:1548` |

Os contraexemplos que a cópia sustenta são de **projeções incompletas**:

- **Episódio perdido:** H1 responde `18:00`, é validada e depois corrigida para `19:00` em T; H2 responde pela primeira vez `19:00` em T. O caminho de resposta permite os mesmos campos atuais `19:00/T/validada=None`, sem criar `RespostaAnterior` na correção ordinária. A história distingue os episódios; os campos atuais não. **Pela regra atual, ambas aguardam admin**: exigir pendências diferentes apenas porque o número do episódio difere seria regra adicional, não demonstrada. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/juizes.py:1150`.

- **Efeito omitido:** H1 valida e planta uma batida viva no slot; H2 valida, mas o plantio é recusado. Ao projetar ambas somente em resposta/validação e numa mesma foto da célula, perde-se a diferença que a pós-condição precisa responder: “efeito existe” versus “efeito não existe”. A consulta independente às batidas separa as histórias; o carimbo de validação não separa. O fluxo possui justamente recusa, prova e devolução. `app/chamados/services/materializacao.py:1207`; `app/chamados/juizes.py:720`; `app/chamados/services/validacao.py:180`.

- **Marco sem vínculo:** dia com duas saídas de intervalo, às `12:00` e `16:00`; uma declaração sem identificador da instância pode referir-se a qualquer uma. As duas intenções não são distinguíveis por motivo/data/texto iguais. O código agrupa por motivo e explicitamente se abstém diante de duas pausas; escolher marcos diferentes como resposta obrigatória, sem vínculo registrado, seria inventar informação. `app/chamados/models.py:2392`; `app/chamados/services/fabrica_da_celula.py:62`; `app/chamados/juizes.py:1107`.

- **Equivalência contextual:** para intervalo fixo às `12:00`, respostas `12:01` e `12:03` podem gerar o mesmo plantio `12:00`, pois o materializador usa a hora do marco. Isso já permite igualdade de efeito nesse contexto; não autoriza equivalência universal entre textos. Também não autoriza tratar tolerância como equivalência transitiva: a regra de classe usa distância limitada. `app/chamados/services/materializacao.py:404`; `app/chamados/services/materializacao.py:411`; `app/chamados/juizes.py:1560`.

Consequentemente, **não seria correto fabricar quatro pares com saídas obrigatoriamente diferentes**: para episódio e marco, a cópia demonstra perda/ambiguidade, mas não uma lei que obrigue saídas diferentes apesar de entradas completas iguais. Essa obrigação permanece **[nao verificado]**.

As decisões atuais são mutáveis nestes pontos:

| Registro | Escritores que alteram ou removem |
|---|---|
| Resposta | Sobrescrita em `responder_pergunta_disputa_core`; limpeza em `reabrir_para_correcao`; exclusão em `remover_motivo`. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1976` |
| `validada_em` | Gravação explícita; limpeza por devolução, correção e reabertura; gravação automática também em `carimbar`. `app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:2591` |
| `materializada_em` | Atribuição pelo núcleo do carimbo; descarimbo individual e em lote. `app/chamados/models.py:2556`; `app/chamados/models.py:2648`; `app/chamados/models.py:2699` |
| Ausência | Edição de tipo/datas/minutos; substituição de estado; reabertura da mesma linha; exclusão de férias e rejeição em lote. `app/ponto/services/ausencia.py:498`; `app/ponto/services/ausencia.py:824`; `app/ponto/services/ausencia.py:1141`; `app/ponto/services/ausencia.py:1262`; `app/ponto/services/ausencia.py:1281` |
| Batida | Alteração de tipo/aprovação/retratação/origem; retratação; exclusão de offline pendente. `app/ponto/registro_batida.py:203`; `app/ponto/registro_batida.py:252`; `app/ponto/registro_batida.py:274`; `app/ponto/registro_batida.py:285` |

**(3) Alimentação e desenho da medida**

| Caminho | Custo que a leitura estática permite afirmar |
|---|---|
| `CargaDaJanela` | Lote real para muitos colaboradores na mesma janela. Base não vazia: **5–7 consultas**, conforme geradoras adicionais e necessidade da última batida; consultas auxiliares dos corpos ficam fora desse subtotal. Logo, parcela base amortizada `5..7 / (N × D)`, não “5–7 por dia”. `app/ponto/turnos.py:2931`; `app/ponto/turnos.py:2943`; `app/ponto/turnos.py:2962`; `app/escala/alimentacao.py:148` |
| Cartório | Por colaborador: batidas da janela, primeira batida e chamados; perguntas vêm em prefetch. Cobertura de ausência é consultada por célula. Grade, veto, teto, C6 e novas passadas acrescentam custo variável. **Não há número total fixo por colab-dia demonstrado.** `app/ponto/services/cartorio.py:117`; `app/ponto/services/cartorio.py:1123`; `app/ponto/services/cartorio.py:1154`; `app/ponto/services/cartorio.py:1306` |
| Comando do cartório | Acrescenta consulta da fila e carregamento de colaborador/células por colaborador. `app/ponto/management/commands/processar_cartorio.py:49` |
| `apurar_por_fatia` | Recebe um colaborador e listas já carregadas; monta motores por callback. Não é o carregador de N colaboradores e seu custo completo depende do callback. `app/ponto/services/fechamento.py:132`; `app/ponto/services/fechamento.py:164` |
| `alimentar_portas` | Faz lote de células, mas lê `qs_vivos()` e `qs_na_fila()` globais; C6 ainda pode consultar por pergunta e app por colaborador. Seu quociente por N×D não representa custo marginal constante. `app/chamados/juizes.py:1261`; `app/chamados/juizes.py:1287`; `app/chamados/juizes.py:1312` |

O script seguinte é **texto proposto, não executado [nao verificado]**. Mede as fontes atuais e mantém a parcela global de portas separada; não mede um feed imutável inexistente nem uma implementação futura de `tarefa_do_dia`. As APIs usadas correspondem aos carregadores acima e às fontes de decisões/efeitos já identificadas. `app/ponto/turnos.py:2936`; `app/chamados/models.py:3179`; `app/chamados/juizes.py:1260`.

Cole no `manage.py shell` já configurado para a sombra e o tenant pretendido. A execução e a configuração desse ambiente permanecem **[nao verificado]**.

```python
from datetime import date, datetime, time, timedelta
from time import perf_counter
from django.db import connection, transaction
from django.db.models import Q
from django.test.utils import CaptureQueriesContext
from django.utils import timezone

from colaboradores.models import Colaborador
from ponto.models import Batida, Ausencia
from chamados.models import (
    ChamadoColaborador, DisputaSupervisao,
    PerguntaDisputa, RespostaAnterior,
)
from ponto.turnos import CargaDaJanela, cobertura_ausencia_periodo
from ponto.precedencia import fatos_do_periodo
from ponto.services.cartorio import marcos_vencidos
from chamados.juizes import alimentar_portas, porta_alcancavel_do_dia

N = 100
INI, FIM = date(2026, 10, 21), date(2026, 11, 5)
AGORA = timezone.now()
assert INI <= FIM
assert connection.vendor == "postgresql"
assert not connection.in_atomic_block

medidas, dados = [], {}

def medir(nome, fn):
    # Apenas instrumentação em memória.
    connection.queries_log.clear()
    t0 = perf_counter()
    with CaptureQueriesContext(connection) as cap:
        valor = fn()
    segundos = perf_counter() - t0
    if len(connection.queries_log) >= connection.queries_limit:
        raise RuntimeError("Captura saturada; não reportar contagem truncada")
    medidas.append((nome, segundos, len(cap)))
    return valor

with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute(
            "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
        )

    colabs = medir("selecao", lambda: list(
        Colaborador.objects.select_related("empresa").order_by("pk")[:N]
    ))
    ids = [c.pk for c in colabs]
    assert ids, "Universo vazio"
    den = len(ids) * ((FIM - INI).days + 1)

    carga = medir("carga_lote", lambda: CargaDaJanela(colabs, INI, FIM))
    cels = [
        cel for c in colabs
        for cel in carga.celulas_de(c.pk, INI, FIM).values()
    ]

    dados["geometria"] = medir("geometria", lambda: {
        c.pk: (
            carga.turnos(c),
            carga.dias_encerrados(c, AGORA),
            carga.dia_das_batidas(c),
        ) for c in colabs
    })

    def decisoes():
        # Vida inteira, explicitamente: não cortar versões anteriores.
        ps = list(PerguntaDisputa.objects.filter(
            disputa__colaborador_id__in=ids
        ).values())
        pids = [p["id"] for p in ps]
        return {
            "perguntas": ps,
            "anteriores": list(RespostaAnterior.objects.filter(
                pergunta_id__in=pids).values()),
            "disputas": list(DisputaSupervisao.objects.filter(
                colaborador_id__in=ids).values()),
            "chamados": list(ChamadoColaborador.objects.filter(
                colaborador_id__in=ids).values()),
        }

    dados["decisoes"] = medir("decisoes_atuais_historico", decisoes)
    pids = [p["id"] for p in dados["decisoes"]["perguntas"]]
    lo = timezone.make_aware(datetime.combine(carga.pad_ini, time.min))
    hi = timezone.make_aware(datetime.combine(
        carga.pad_fim + timedelta(days=1), time.min
    ))

    dados["efeitos"] = medir("efeitos_brutos", lambda: {
        "batidas": list(Batida.objects.filter(
            Q(colaborador_id__in=ids, timestamp__gte=lo, timestamp__lt=hi)
            | Q(pergunta_origem_id__in=pids)
        ).values()),
        "ausencias": list(Ausencia.objects.filter(
            colaborador_id__in=ids).values()),
    })

    def juizos():
        out = {}
        for c in colabs:
            out[c.pk] = {
                "fatos": fatos_do_periodo(c, INI, FIM),
                "cobranca": cobertura_ausencia_periodo(
                    c, INI, FIM, para="cobranca"),
                "folha": cobertura_ausencia_periodo(
                    c, INI, FIM, para="folha"),
                "teto": {
                    cel.pk: marcos_vencidos(c, cel, cel.data, AGORA)
                    for cel in carga.celulas_de(c.pk, INI, FIM).values()
                },
            }
        return out

    dados["juizos"] = medir("precedencia_cobertura_teto", juizos)

    def portas():
        alim = alimentar_portas([cel.pk for cel in cels], colabs=colabs)
        return {
            cel.pk: porta_alcancavel_do_dia(cel.pk, alimentacao=alim)
            for cel in cels
        }

    dados["portas"] = medir("portas_com_universo_global", portas)

    print({"N": len(ids), "colab_dias": den, "celulas": len(cels)})
    for nome, segundos, queries in medidas:
        print({
            "etapa": nome, "segundos": segundos, "queries": queries,
            "ms_por_colab_dia": 1000 * segundos / den,
            "queries_por_colab_dia": queries / den,
        })
    print({
        "total_segundos": sum(s for _, s, _ in 

[truncado em 20000 de 31108 caracteres]

## 06/11 21:40 · sessao 361072d4a9c5 · - · - · origem=Ronald (tmux)

Item (5), somente leitura; números de produção/DIFF efetivo das competências 10 e 11: **[nao verificado]**.

| Leitor/efeito | O que muda ao re-chavear | Referência |
|---|---|---|
| Fio: data e aviso de divergência | `ev=p.data_evento`; compara com `dia_do_fato(ch, fortes=True)` do chamado que lista | `app/chamados/services/fio.py:77-88` |
| Modal do fio | Mostra data da pergunta e data do chamado quando divergentes; senão “Refere-se ao dia…” | `app/templates/chamados/partials/modal_fio.html:161-182` |
| Cartão atendimento | Exibe diretamente `p.data_evento` | `app/templates/chamados/partials/_card_atendimento.html:87-90` |
| Ficha do dia | Pergunta passa de um dia para outro porque consulta `data_evento=data` | `app/ponto/services/dia_do_colab.py:64-71` |
| Prévia da folha | Universo da competência e agrupamento por dia usam `data_evento`; sinal “resposta”, “pergunta viva” ou “nada” acompanha novo dia | `app/folha/previa.py:64-67`, `app/folha/previa.py:95`, `app/folha/previa.py:139-146` |
| Diagnóstico de pergunta no app | “pergunta viva em outro chamado” e pergunta encerrada do marco são procuradas no dia | `app/chamados/services/pergunta_no_app.py:92-95`, `app/chamados/services/pergunta_no_app.py:109-111` |
| Cobrança | Lista de dias pendentes usa `data_evento` | `app/chamados/services/cobranca.py:81-90` |
| Decisão administrativa do dia | Seleção de perguntas a fechar é por `data_evento=data` | `app/ponto/services/veredito_celula.py:174-178` |
| Validação e folha por evento | Competência agendada para recálculo recebe `perg.data_evento` | `app/chamados/services/validacao.py:395-408` |
| Expiração/veredito persistido | `janela_expira_em` deriva de `data_evento`; reapontamento exige relavra no mesmo save | `app/chamados/veredito_lavrado.py:95-118`, `app/chamados/veredito_lavrado.py:227-231`, `app/chamados/services/fio.py:23-25` |
| Outbox | `data_evento` é campo que acorda célula; busca prefere FK `p.celula`, só depois `(colab,data_evento)` | `app/chamados/models.py:2682-2710` |

Para FM/DiaPago, há **efeito indireto a medir**, sem prova fornecida de alteração monetária: fechamento alimenta o motor com batidas, escalas e células, e recebe `resultado`; a lavratura do DiaPago recebe esse mesmo resultado e os mapas da grade. (`app/ponto/services/fechamento.py:725-747`, `app/ponto/services/fechamento.py:1175-1211`) A migração da data da pergunta, por si só, não demonstra diferença nas horas. **[nao verificado quanto ao resultado de produção]**

DIFF FM sem escrever: porta real `recalcular_fechamento_mes(10/11, 2026, ..., somente_leitura=True, lavrar_dias_pagos=False, retornar_objeto=True, coletar_furos={})`; retorna objeto destacado e furos derivados pelo mesmo percurso. (`app/ponto/services/fechamento.py:439-474`, `app/ponto/services/fechamento.py:1112-1120`, `app/ponto/services/fechamento.py:1293-1295`) `fechamento_lido` já usa esse modo para uma pessoa. (`app/ponto/services/fechamento.py:1743-1745`)

**Não usar `lavrar_dias_pagos=True` em sonda só leitura:** ele chama o lavrador mesmo com `somente_leitura=True`, e o lavrador termina em `delete` e `bulk_create`. (`app/ponto/services/fechamento.py:1175-1185`, `app/ponto/services/dia_pago.py:357-358`) Não há retorno de projeção DiaPago no retorno FM acima; a comparação por dia pode interceptar a chamada real a `dia_pago.lavrar`, deixando executar sua derivação em memória e substituindo as operações finais de persistência — proposta de ensaio, **[nao verificado por execução]**. Os campos monetários canônicos são `CAMPOS_DIA` e `CAMPOS_AJUSTE`; ajuste inclui DSR e banco, e a soma existente arredonda os totais a duas casas. (`app/ponto/services/dia_pago.py:34-44`, `app/ponto/services/dia_pago.py:348-356`, `app/ponto/services/dia_pago.py:556-561`)

Competências 10/11 devem ser recortadas pela janela da empresa, não pelo mês civil: `folha.export.janela_competencia` delega a `janela_fechamento`, que usa `corte_da_empresa`. (`app/folha/export.py:47-51`, `app/ponto/janelas.py:36-63`) Para cada par antigo/novo, o roteamento existente é `competencias_do_fato` → `competencias_do_intervalo` → `janela_atual(cur, empresa)`, sem uma regra própria de dia. (`app/ponto/services/fechamento.py:1578-1588`, `app/ponto/services/dia_pago.py:711-716`) Janela concreta de cada empresa, deltas FM, deltas DiaPago, retenção/exportação e zeros nas competências 10/11: **[nao verificado]**.

## 06/11 21:37 · sessao 86792b3bca5e · - · - · origem=Ronald (tmux)

Censo conferido, sem executar Django nem escrever arquivos. Não há `AGENTS.md` listado por `rg --files -g AGENTS.md`; instruções eventualmente fora da árvore permanecem [nao verificado].

**Unidade recomendada: funções/portas que persistem alteração após o nascimento**, sem somar wrappers e auxiliares que só alteram memória. Nesse recorte são **16: 1 Chamado + 3 Disputa + 12 Pergunta**. A soma inclui os campos de resposta/validação que hoje decidem pendência, não apenas um campo chamado `status`. Fontes abaixo.

| Entidade | Porta persistente | Campo/efeito |
|---|---|---|
| Chamado | `_gravar_estado` | `status_local`, `resolvido_em`, `via_encerramento`; atribuições em `app/chamados/models.py:222`, `app/chamados/models.py:238`, `app/chamados/models.py:242`. |
| Disputa | `fechar` | `fechada_em`, autor, resolução, motivo: `app/chamados/models.py:1861`. |
| Disputa | `reabrir_admin` | Limpa os quatro anteriores: `app/chamados/models.py:2063`. |
| Disputa | `reabrir_sistema` | Limpa os quatro anteriores: `app/chamados/models.py:2087`. |
| Pergunta | `responder_pergunta_disputa_core` | Substitui resposta/instante; desfaz validação anterior: `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1295`, save em `app/chamados/services/disputa_emissao.py:1301`. |
| Pergunta | `reabrir_para_correcao` | Zera texto, resposta e validação: `app/chamados/services/disputa_emissao.py:1393`, persistência em `app/chamados/services/disputa_emissao.py:1414` ou `app/chamados/services/disputa_emissao.py:1416`. |
| Pergunta | `responder_automaticamente` | Escreve texto/instante: `app/chamados/services/disputa_emissao.py:1433`. |
| Pergunta | `carimbar_resposta_pela_trilha` | Reconstrói instante: `app/chamados/services/disputa_emissao.py:1450`. |
| Pergunta | `_carimbar_validacao` | Escreve validação/autor: `app/chamados/services/validacao.py:131`. |
| Pergunta | `devolver_validacao_recusada` | Limpa validação/autor: `app/chamados/services/validacao.py:188`. |
| Pergunta | `carimbar` | Carimbo materialização/via e, opcionalmente, resposta/validação: `app/chamados/models.py:2586`, `app/chamados/models.py:2589`, `app/chamados/models.py:2592`, save em `app/chamados/models.py:2609`. |
| Pergunta | `carimbar_identidade` | Carimbo materialização/via: `app/chamados/models.py:2636`, save em `app/chamados/models.py:2644`. |
| Pergunta | `descarimbar` | Limpa carimbo/via: `app/chamados/models.py:2648`, save em `app/chamados/models.py:2652`. |
| Pergunta | `descarimbar_em_lote` | Limpa carimbo/via e outros campos recebidos: `app/chamados/models.py:2699`. |
| Pergunta | `encerrar_por_fato_no_espelho` | Escreve somente `via_resolucao`: `app/chamados/models.py:2709`. |
| Pergunta | `relavrar` | **Outro escritor de `via_resolucao`**, fora da porta do carimbo: `app/chamados/services/relavra_via.py:123`, save em `app/chamados/services/relavra_via.py:125`. |

Os nascimentos explícitos de Chamado acrescentam **duas portas**, resultando em **18 no recorte com esses nascimentos**: `abrir` fornece `status_local` nos defaults em `app/chamados/models.py:899`; `criar` o grava no create em `app/chamados/models.py:1056`. Nascimentos de pergunta/disputa com valores vazios por default devem ficar em categoria separada para não comparar unidades diferentes; pergunta nasce em `app/chamados/models.py:2476`, defaults de resposta em `app/chamados/models.py:2227`, default de disputa aberta em `app/chamados/models.py:1687`.

Não somar como escritores persistentes: `_escrever_carimbo` é auxiliar em memória (`app/chamados/models.py:2556`); `_desfazer_em_memoria` restaura memória (`app/chamados/models.py:2616`); `_vivas_entre` troca/restaura status em memória (`app/chamados/models.py:339`, `app/chamados/models.py:354`); `_universo_da_ressuscitacao` altera cópia (`app/chamados/models.py:1443`, `app/chamados/models.py:1456`). O selo AST de status declara limites de resolução dinâmica em `app/chamados/tests/test_selo_status_so_pelas_portas.py:15`; exclusão absoluta de escritores indiretos permanece [nao verificado].

**Extras que influem na pendência/exibição e precisam aparecer fora desse subtotal:**

| Campos | Escritores | Leitores |
|---|---|---|
| `pendencia_admin` | `_materializar_resposta_hora`, único sítio encontrado: `app/chamados/services/materializacao.py:825`. | `explicador` dá dono admin antes de consultar efeito: `app/chamados/services/explicador.py:85`, `app/chamados/services/explicador.py:93`. |
| `recusa_motivo` | Cinco funções encontradas: `responder_pergunta_disputa_core` em `app/chamados/services/disputa_emissao.py:1340`; `_materializar_resposta_hora` em `app/chamados/services/materializacao.py:825`; `_carimbar_recusa` em `app/chamados/services/materializacao.py:1077`; `recusar_pergunta` em `app/chamados/services/validacao.py:545`; `carimbar_identidade` em `app/chamados/models.py:2642`. | Motivo da pendência no explicador: `app/chamados/services/explicador.py:90`; habilita devolução de validação em `app/chamados/services/validacao.py:184`. |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar` produz/grava em `app/chamados/veredito_lavrado.py:227`; `gravar_lavras_em_lote` persiste em `app/chamados/veredito_lavrado.py:207`; command também **produz** os valores em `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, mas persiste pela porta de lote em `app/chamados/management/commands/backfill_veredito_lavrado.py:143`. | `esta_stale` em `app/chamados/veredito_lavrado.py:179`; crédito admin exclui materializada + VL_SIM em `app/chamados/services/raiox.py:133`. |
| `silenciado_ate/por/motivo`, `resgatado_em` | `silenciar` em `app/chamados/services/arquivo.py:166`; `levantar` em `app/chamados/services/arquivo.py:201`. | Painel exclui silêncio futuro em `app/chamados/services/painel.py:186`; contrato declara que não muda status nem apuração em `app/chamados/services/arquivo.py:13`. |
| `data_conferida_em/por` | `conferir_data` em `app/chamados/services/validacao.py:617`. | Materializador barra divergência sem conferência em `app/chamados/services/materializacao.py:1168`. |
| `chamado_destino`, `data_evento` | `PerguntaDisputa.abrir` reaponta em `app/chamados/models.py:2450`, salva em `app/chamados/models.py:2474`, nasce em `app/chamados/models.py:2476`; `fio.reapontar_resolvedora` preenche data vazia em `app/chamados/services/fio.py:834`, reaponta/salva em `app/chamados/services/fio.py:845`. | Âncora do fio em `app/chamados/services/fio.py:116`; crédito destino/pai em `app/chamados/services/raiox.py:54`. |

**Leitores do núcleo:** `status_local` alimenta universo e fila em `app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:187`; `resolvido_em/via_encerramento` sustentam veto de reabertura sem fato novo em `app/chamados/models.py:1303`; `fechada_em` define `.aberta` em `app/chamados/models.py:1765`, restringe badge em `app/chamados/models.py:1737` e universo PWA em `app/chamados/services/fio.py:213`. A API busca perguntas sem o filtro de disputa aberta em `app/chamados/services/pergunta_no_app.py:46`.

Resposta/validação/materialização/via são lidas pelo C6 em `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`; `.estado` deriva rótulo desses campos em `app/chamados/models.py:2499`; o materializador exige respondida+validada+materializada nula em `app/chamados/services/materializacao.py:1344`.

**Imutabilidade adicional importante:** `DisputaSupervisao.remover_motivo` **apaga a pergunta**, inclusive respondida: operação efetiva em `app/chamados/models.py:1976`, comportamento documentado em `app/chamados/models.py:1960`. Isso não está no subtotal de atribuições de campos; é uma porta adicional de remoção da própria decisão. `reabrir_para_correcao` preserva versão anterior em `RespostaAnterior` antes de limpar em `app/chamados/services/disputa_emissao.py:1406`; a resposta humana ordinária simplesmente sobrescreve o campo em `app/chamados/services/disputa_emissao.py:1282`.

Para v3/v4, contagem **implementada** de escritores novos permanece [nao verificado]. Pelo desenho do usuário, v3 pretende um escritor de estado da obrigação; v4 pretende zero escritores de estado derivado da tarefa. Isso não significa apagar os escritores de decisões, efeitos, arquivos ou cache; as fontes acima mostram que essas categorias hoje compartilham campos e portas.

## 06/11 21:36 · sessao d0a16a5d9125 · - · - · origem=Ronald (tmux)

Fontes conferidas para entradas e custo:

| Entrada/função | Retorno, universo e unidade |
|---|---|
| `celulas_do_periodo(ids,ini,fim)` | `{(colaborador_id,date): CelulaDia}`, intervalo inclusivo, todos os colaboradores explicitados, uma consulta. Não exige situação ativa. `app/escala/alimentacao.py:54-78`. |
| `fatos_do_dia` / `fatos_do_periodo` | Dicionário com tipo vencedor, faixa, rótulo, suprimidos, previsto e fatos encontrados; precedência de ausências/folgas/feriados, por datas do período. Não é dump completo da célula. `app/ponto/precedencia.py:242-279`. |
| `marcos_turno` | `[(tipo,datetime_aware)]`, ancorado no dia do turno e avançando D+1 quando necessário; com `ec`, lê DNA congelado. `app/escala/servico_jornada.py:61-86`. |
| `marcos_vencidos` | `(vencidos,pendentes)`, listas `(tipo,'HH:MM')`, compara instante do marco + teto com `agora`; usa `escala_vigente`. `app/ponto/services/cartorio.py:831-848`. |
| `vencidos_no_instante` | Conjunto de marcos vencidos; `None` para passado sem marcos, conjunto vazio para hoje/futuro sem marcos. `app/chamados/services/fabrica_da_celula.py:15-31`. |
| `motivos_a_perguntar` | Lista de candidatos ainda não perguntados; exige ativo, dia cobrável, teto, veredito e parcial; exclui motivos já presentes em perguntas históricas. Portanto não responde pendência universal. `app/chamados/services/fabrica_da_celula.py:74-126`. |
| `ausencia_cobre` | Ausência vencedora ou `None`, por dia inclusivo; cobrança exclui rejeitada, folha exige aprovada; maior `(data,pk)` vence. `app/ponto/turnos.py:2509-2557`. |
| `cobertura_ausencia_periodo` | `{date: Ausencia}`, mesma cobertura/desempate para todo período, uma consulta por colaborador/efeito. `app/ponto/turnos.py:2733-2771`. |
| `_marcos_faltantes` | `(estado,[(dia,posição,tipo,hora)])`; distingue sem célula/fora do vínculo; usa dia do chamado e saída noturna da véspera, portanto requer cuidado ao interpretar como dia canônico do turno. `app/chamados/services/declaracao_texto.py:65-127`. |
| `porta_alcancavel_do_dia` | `None` ou `{rota,fila,verbos,fio}`; portas de fila/pergunta no app, sem acesso manual e Resolver dia. Múltiplos verbos são possíveis; responsável agregado prioriza cobrar/colab, validar/admin. `app/chamados/juizes.py:1323-1387`. |
| `alimentar_portas` | Carrega por lote, mas chama `qs_vivos()` e `qs_na_fila()` **globais**; C6 consulta por pergunta e app por colaborador. Não é custo limitado apenas aos N escolhidos. `app/chamados/juizes.py:1260-1319`. |
| Decisões atuais | Pergunta guarda resposta/instantes, conferência, vias; `RespostaAnterior` guarda a resposta retirada na reabertura. Essa última tem FK com cascade, não proteção absoluta contra exclusão. `app/chamados/models.py:2216-2241`; `app/chamados/models.py:3165-3188`. |

O lookup registra esses domínios separadamente e declara lacunas de fechamento, meia-noite/prazos e competência fechada. `app/docs/JUIZES.md:20-105`. Não encontrei nas fontes abertas uma alimentação imutável completa de decisões/atos; o script abaixo mede as fontes **atuais**, sem afirmar que elas suprem essa falta. `app/chamados/models.py:2227-2241`; `app/chamados/models.py:3165-3188`; `[nao verificado]` quanto à existência fora do escopo aberto.

Custo estático:

- `CargaDaJanela` é o carregador existente de muitos colaboradores e uma mesma janela: células, escalas, ata, batidas e resultados de turnos/dias encerrados/dia das batidas. Carga base não vazia: **5–7 consultas**, conforme vínculos adicionais e consulta da última batida; não inclui consultas auxiliares disparadas nos corpos. O comentário declara candidatos “2”, mas a segunda consulta só ocorre se faltarem geradoras. `app/ponto/turnos.py:2922-3009`; `app/escala/alimentacao.py:148-159`.
- Cartório não tem custo fixo por colab-dia: batidas da janela 1/colab, primeira batida 1/colab, chamados 1/colab + prefetch de perguntas quando há chamados, ausência 1/célula, mais grade/cadastro/veto/C6/teto e possíveis novas passadas. `app/ponto/services/cartorio.py:117-118`; `app/ponto/services/cartorio.py:1091-1126`; `app/ponto/services/cartorio.py:1154-1155`; `app/ponto/services/cartorio.py:1179`; `app/ponto/services/cartorio.py:1303-1306`; `app/ponto/services/cartorio.py:969-973`.
- O comando soma fila 1 + colaborador 1 e células 1 por colaborador. `app/ponto/management/commands/processar_cartorio.py:49-65`.
- `apurar_por_fatia` recebe **um colaborador** e listas carregadas; callback monta motor por fatia, portanto não é alimentação de muitos colaboradores nem tem custo total fixo independente do callback. `app/ponto/services/fechamento.py:132`; `app/ponto/services/fechamento.py:164-178`.

Script proposto, **não executado** `[nao verificado]`, para colar no `manage.py shell` já apontado à sombra/schema desejado. Mede denominador `N efetivo × dias`, células reais separadamente; mantém consultas globais de portas em estágio próprio. Não chama julgar/emissor/apurar/materializar. As APIs utilizadas foram abertas nos endereços acima; campos/imports de efeitos: `app/ponto/models.py:40`; `app/ponto/models.py:660`; histórico: `app/chamados/models.py:3179`.

```python
from datetime import date, datetime, time, timedelta
from time import perf_counter
from django.db import connection, transaction
from django.db.models import Q
from django.test.utils import CaptureQueriesContext
from django.utils import timezone

from colaboradores.models import Colaborador
from ponto.models import Batida, Ausencia
from chamados.models import (
    ChamadoColaborador, DisputaSupervisao, PerguntaDisputa,
    RespostaAnterior,
)
from ponto.turnos import CargaDaJanela, cobertura_ausencia_periodo
from ponto.precedencia import fatos_do_periodo
from ponto.services.cartorio import marcos_vencidos
from chamados.juizes import alimentar_portas, porta_alcancavel_do_dia

N = 100
INI, FIM = date(2026, 10, 21), date(2026, 11, 5)
AGORA = timezone.now()
assert INI <= FIM
assert connection.vendor == "postgresql"

medidas, dados = [], {}
def medir(nome, fn):
    t0 = perf_counter()
    with CaptureQueriesContext(connection) as cap:
        valor = fn()
    medidas.append((nome, perf_counter() - t0, len(cap)))
    return valor

with transaction.atomic():
    # Barreira contra efeitos persistidos das funções chamadas.
    with connection.cursor() as cur:
        cur.execute(
            "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ READ ONLY"
        )

    colabs = medir(
        "selecao",
        lambda: list(
            Colaborador.objects.select_related("empresa").order_by("pk")[:N]
        ),
    )
    ids = [c.pk for c in colabs]
    n = len(ids)
    assert n, "Universo vazio"
    dias = (FIM - INI).days + 1

    carga = medir("carga_lote", lambda: CargaDaJanela(colabs, INI, FIM))
    cels = [
        cel for c in colabs
        for cel in carga.celulas_de(c.pk, INI, FIM).values()
    ]

    def geometria():
        return {
            c.pk: (
                carga.turnos(c),
                carga.dias_encerrados(c, AGORA),
                carga.dia_das_batidas(c),
            ) for c in colabs
        }
    dados["geometria"] = medir("geometria_real", geometria)

    def decisoes():
        # Vida inteira: a identidade/reabertura atual consulta histórico.
        ps = list(PerguntaDisputa.objects.filter(
            disputa__colaborador_id__in=ids
        ).values())
        pids = [p["id"] for p in ps]
        return {
            "perguntas": ps,
            "anteriores": list(RespostaAnterior.objects.filter(
                pergunta_id__in=pids).values()),
            "disputas": list(DisputaSupervisao.objects.filter(
                colaborador_id__in=ids).values()),
            "chamados": list(ChamadoColaborador.objects.filter(
                colaborador_id__in=ids).values()),
        }
    dados["decisoes"] = medir("decisoes_atuais_historico", decisoes)
    pids = [p["id"] for p in dados["decisoes"]["perguntas"]]

    lo = timezone.make_aware(datetime.combine(carga.pad_ini, time.min))
    hi = timezone.make_aware(datetime.combine(
        carga.pad_fim + timedelta(days=1), time.min
    ))
    def efeitos():
        return {
            # Inclui retratadas e efeitos originados nas perguntas históricas.
            "batidas": list(Batida.objects.filter(
                Q(colaborador_id__in=ids, timestamp__gte=lo, timestamp__lt=hi)
                | Q(pergunta_origem_id__in=pids)
            ).values()),
            "ausencias": list(Ausencia.objects.filter(
                colaborador_id__in=ids).values()),
        }
    dados["efeitos"] = medir("efeitos_brutos", efeitos)

    def juizos():
        por_colab = {}
        por_celula = {}
        for c in colabs:
            por_colab[c.pk] = {
                "fatos": fatos_do_periodo(c, INI, FIM),
                "cobranca": cobertura_ausencia_periodo(
                    c, INI, FIM, para="cobranca"),
                "folha": cobertura_ausencia_periodo(
                    c, INI, FIM, para="folha"),
            }
            for cel in carga.celulas_de(c.pk, INI, FIM).values():
                por_celula[cel.pk] = marcos_vencidos(
                    c, cel, cel.data, AGORA
                )
        return por_colab, por_celula
    dados["juizos"] = medir("precedencia_cobertura_teto", juizos)

    def portas():
        alim = alimentar_portas([cel.pk for cel in cels], colabs=colabs)
        return {
            cel.pk: porta_alcancavel_do_dia(cel.pk, alimentacao=alim)
            for cel in cels
        }
    dados["portas"] = medir("portas_inclui_painel_global", portas)

    den = n * dias
    print({"N": n, "dias": dias, "colab_dias": den,
           "celulas_reais": len(cels)})
    for nome, segundos, queries in medidas:
        print({"etapa": nome, "segundos": segundos, "queries": queries,
               "ms_por_colab_dia": 1000 * segundos / den,
               "queries_por_colab_dia": queries / den})
    print({"total_segundos": sum(s for _, s, _ in medidas),
           "total_queries": sum(q for _, _, q in medidas)})
```

Limites da medida: acesso aos dados e execução permanecem `[nao verificado]`; fontes são snapshots atuais com histórico parcial, não decisões imutáveis completas; ordem dos estágios aquece caches do processo; o estágio de portas contém trabalho global e seu quociente por colab-dia não deve ser extrapolado como custo marginal constante. `app/chamados/models.py:2227-2241`; `app/chamados/models.py:3165-3188`; `app/chamados/juizes.py:1260-1319`.

## 06/11 21:36 · sessao e9b1aef4cf79 · - · - · origem=Ronald (tmux)

**(2) Derivabilidade e mutabilidade — evidências**

- As decisões atuais **não são imutáveis**: resposta humana substitui `resposta_colab/respondida_em` e desfaz validação anterior; isso grava a nova resposta, sem criar `RespostaAnterior` nesse caminho. `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1301`.
- Reabertura arquiva texto, instante da resposta e validação em `RespostaAnterior`, depois limpa os campos atuais; a versão não registra o efeito materializado anterior. `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1406`, `app/chamados/services/disputa_emissao.py:1414`.
- O arquivo de resposta também pode desaparecer: `DisputaSupervisao.remover_motivo` apaga a pergunta inclusive respondida; `RespostaAnterior.pergunta` usa `CASCADE`. `app/chamados/models.py:1960`, `app/chamados/models.py:1976`, `app/chamados/models.py:3179`.
- Validação ganha instante pela porta `_carimbar_validacao`; devolução da recusa limpa instante/autor mantendo a resposta. `app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:187`.
- O carimbo é mutável: atribuição central `_escrever_carimbo`; limpeza individual e em lote; reabertura por resposta/competência/fantasma passa pelas inversas. `app/chamados/models.py:2556`, `app/chamados/models.py:2646`, `app/chamados/models.py:2692`, `app/chamados/models.py:2723`, `app/chamados/models.py:2735`, `app/chamados/models.py:2751`.
- `carimbar(resposta='validar')` também pode escrever `respondida_em`, `validada_em` e autor, além de `materializada_em/via`; portanto validação não tem apenas o sítio explícito em `validacao.py`. `app/chamados/models.py:2585`, `app/chamados/models.py:2587`, `app/chamados/models.py:2591`.
- Ausência muda tipo, datas e minutos; status é substituído pela porta; rejeitada é reaberta na **mesma linha**; férias admitem exclusão física e rejeição por `update`. `app/ponto/services/ausencia.py:498`, `app/ponto/services/ausencia.py:509`, `app/ponto/services/ausencia.py:824`, `app/ponto/services/ausencia.py:835`, `app/ponto/services/ausencia.py:1141`, `app/ponto/services/ausencia.py:1262`, `app/ponto/services/ausencia.py:1281`.
- Batida admite alteração de tipo/aprovação/retratação/`pergunta_origem`; retratar escreve `aprovada=False/retratada_em`; existe DELETE para rejeição de offline pendente. `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:252`, `app/ponto/registro_batida.py:274`, `app/ponto/registro_batida.py:285`.

**Distinção lógica indispensável:** não se demonstra necessidade de tabela própria apenas mostrando que snapshots perdem história. Com decisões completas, ordenadas e imutáveis, uma função pode reduzir a sequência; o que falta hoje é essa fonte completa. Contraexemplo com as **mesmas entradas completas e imutáveis**, mas saída obrigatoriamente diferente segundo a lei vigente: `[nao verificado]`. Não inventar esse requisito para justificar v3. A perda efetiva de informação nos snapshots está demonstrada pelos escritores/substituições/exclusões acima.

**Episódio:** duas histórias podem acabar com os mesmos campos atuais: H1 responde 18:00, valida, corrige para 19:00 em T; H2 responde pela primeira vez 19:00 em T. Ambas terminam `resposta_colab=19:00/respondida_em=T/validada_em=None`; o caminho de correção não cria `RespostaAnterior`. Isso impede contar a história anterior usando só a pergunta atual; **não prova que a pendência atual deva diferir** — ambas aguardam admin pela regra atual. `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1162`.

**Marco:** existem motivos para início/fim/ida/volta; porém o próprio juiz se abstém em dia de duas pausas porque não sabe qual luz corresponde à resposta. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/juizes.py:1103`, `app/chamados/juizes.py:1107`. Se a entrada tiver apenas motivo e dia, escolher entre duas instâncias não está demonstrado; se a decisão carregar a identidade do marco, não há argumento para exigir estado separado. A pergunta nova necessária seria: “qual instância de marco esta decisão responde, inclusive depois de reescrita do DNA?” `[nao verificado]` quanto à existência de juiz geral para isso.

**Equivalência:** há concordância resposta×batida, não juiz geral resposta×resposta. O juiz usa classe por delta de cinco minutos, luz do marco e dia não acusador; devolve desconhecido com duas pausas. `app/chamados/juizes.py:1087`, `app/chamados/juizes.py:1107`, `app/chamados/juizes.py:1128`. No intervalo fixo, respostas diferentes como 12:01 e 12:03 podem produzir **o mesmo plantio 12:00**, pois o materializador substitui hora respondida pela hora do marco. `app/chamados/services/materializacao.py:404`, `app/chamados/services/materializacao.py:408`, `app/chamados/services/materializacao.py:414`. Não tratar tolerância como equivalência transitiva: 12:00↔12:04↔12:08 pode satisfazer pares vizinhos sem satisfazer extremos; formalização geral dessa equivalência seria pergunta nova `[nao verificado]`.

**Pós-condição:** para HORA/data-hora existe prova externa; o juiz inteiro não é independente de carimbo, porque vias sem testemunha e escolha leem `materializada_em`. `app/chamados/juizes.py:647`, `app/chamados/juizes.py:672`, `app/chamados/juizes.py:718`. A função pura pode conferir efeitos se a alimentação incluir fatos atuais externos; só a ata/cache da célula e as decisões não demonstram a existência atual da linha de efeito. `app/chamados/juizes.py:720`, `app/chamados/juizes.py:774`.

**(4) Estado dos efeitos e consultas que o provam**

| Efeito | Escritor | Prova sem `materializada_em` |
|---|---|---|
| Batida plantada/reutilizada | Materializador chama `registrar_batida`; depois anota `pergunta_origem` inclusive no reuso. `app/chamados/services/materializacao.py:901`, `app/chamados/services/materializacao.py:921`. INSERTs ficam em `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`. | Derivar slot pela mesma função; selecionar `p.batidas_plantadas.filter(retratada_em__isnull=True)` e verificar `_plantio_na_hora_do_slot(p, ts, qs)`; para data-hora compara instante±20min; hora admite D−1/D/D+1 com hora local. `app/chamados/juizes.py:718`, `app/chamados/juizes.py:720`, `app/chamados/juizes.py:242`, `app/chamados/juizes.py:260`, `app/chamados/juizes.py:265`. |
| Ausência declarada | `criar_ausencia` com colaborador/data/tipo/chamado; INSERT em `a.save()`. `app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:418`. | Consultar ausência por colaborador/tipo/data; provar cobertura/estado exigido pelos juízes de ausência. A chave utilizada pelo escritor é documentada em `app/chamados/services/materializacao.py:595`; query existente por essa chave em `app/ponto/services/ausencia.py:247` e `app/ponto/services/ausencia.py:254`. “Existe ausência” não equivale a “está aprovada”: atestado nasce aberto enquanto falta pode nascer aprovada. `app/chamados/services/materializacao.py:579`. |
| Chamado da ausência | Gate `abrir` por colaborador + módulo `ausencia_colab` + `contexto_json.ausencia_id`. `app/ponto/services/ausencia.py:659`. | Consultar exatamente a identidade usada no gate e, separadamente, classificar seu estado pelos conjuntos do motor; existência não prova porta acessível nem ausência decidida. Identidade em `app/ponto/services/ausencia.py:660`. |
| Absorção de perguntas pela ausência | Carimba irmãs do dia com o mesmo `agora`. `app/chamados/services/materializacao.py:529`, `app/chamados/services/materializacao.py:549`. | **Hoje não há ligação suficiente sem o carimbo:** ausência não tem FK inversa para as perguntas absorvidas; a recuperação agrupa `(disputa,data_evento,materializada_em)` e exige escape correspondente; exclui perguntas com batida plantada viva. `app/chamados/services/materializacao.py:588`, `app/chamados/services/materializacao.py:605`, `app/chamados/services/materializacao.py:609`, `app/chamados/services/materializacao.py:616`. Preservar um identificador imutável do ato de absorção é necessário para reproduzir esse comportamento, sem usar timestamp como estado de pendência. |

**Armadilha:** `_batida_existente_na_janela` é guarda de idempotência: inclui batida retratada e não exige aprovação; não é prova isolada de efeito vivo. `app/chamados/juizes.py:227`, `app/chamados/juizes.py:235`. O juiz do efeito acrescenta explicitamente `retratada_em is None`; a distinção entre os dois usos é explicada no código. `app/chamados/juizes.py:768`, `app/chamados/juizes.py:774`.

**Fechamento atual:** validação mata C6 imediatamente; HORA recebe defesa adicional de plantio, prova externa e rollback; escolha deliberadamente pode resolver sem batida. `app/chamados/juizes.py:1135`, `app/chamados/services/materializacao.py:1195`, `app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1214`, `app/chamados/services/materializacao.py:1038`. Logo, a pós-condição v4 precisa ser definida **por espécie de efeito/decisão**, aproveitando esses juízes; uma exigência universal de “batida existe” mudaria a lei para escolhas/ausências. `app/chamados/services/materializacao.py:1046`, `app/chamados/services/materializacao.py:1052`.

## 06/11 21:31 · sessao af15cd47f7f8 · FIO-COM-ASSENTO-E0-V3 · leitura · origem=Code

| Letra | Resultado da conferência |
|---|---|
| **A** | A impressão mistura FATO, AVALIAÇÃO e TAREFA. **Não é canônica nem independente da ordem recebida.** `app/ponto/services/cartorio.py:452`, `app/ponto/services/cartorio.py:522`. |
| **B** | O teto já participa da avaliação e da impressão: vencimento por instante, corte das lâmpadas pendentes e indicador de turno em curso. Precisa sobreviver na **versão da avaliação**. `app/ponto/services/cartorio.py:831`, `app/ponto/services/cartorio.py:1348`, `app/ponto/services/cartorio.py:1378`. |
| **C** | Intervalo e saída final distinguem-se por **motivos diferentes**; a identidade geral não contém MARCO nem instância. Não achei unicidade parcial de tarefa aberta; **há locks**, mas a proteção é incompleta. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:2392`, `app/chamados/models.py:863`. |
| **D** | Hoje, `validada_em` já mata a pergunta para o juiz. O caminho de HORA acrescenta verificação do efeito e devolução da recusa ao admin. `materializada_em` também identifica absorções e encerramentos sem batida; não pode virar simplesmente “tarefa concluída”. `app/chamados/juizes.py:1135`, `app/chamados/services/validacao.py:422`, `app/chamados/services/materializacao.py:585`. |
| **E** | Há cobertura parcial dos dez casos; faltam provas específicas para a sequência noturna completa e duas transições simultâneas de pergunta/disputa. Os testes próximos estão discriminados abaixo. `app/chamados/tests/test_fio_intervalo.py:62`, `app/chamados/tests/test_plantio_intervalo_overnight.py:27`, `app/chamados/tests/test_c2_corrida_do_abrir.py:94`; cobertura exata faltante `[nao verificado]`. |

Vínculo Git da cópia ao hash informado, resultados de execução dos testes e estado operacional de produção: **[nao verificado]**. A cobertura abaixo significa teste encontrado e lido. [nao verificado]

**A — Insumos, mistura e consumidores.** A classificação abaixo aplica as três naturezas propostas aos argumentos efetivamente serializados; a função atual produz uma única impressão para todos eles. `app/ponto/services/cartorio.py:443`, `app/ponto/services/cartorio.py:522`.

| Campo serializado | Natureza no desenho proposto | O que entra hoje |
|---|---|---|
| `bs_janela[].pk` | FATO: identidade do registro | PK da batida. `app/ponto/services/cartorio.py:453`. |
| `bs_janela[].tipo` | FATO | Tipo gravado. `app/ponto/services/cartorio.py:453`. |
| `bs_janela[].timestamp` | FATO | `isoformat()` do instante recebido. `app/ponto/services/cartorio.py:453`. |
| `cob_status` | **AVALIAÇÃO da cobertura escolhida**; o status da ausência de origem é um dado dessa fonte | Apenas o status retornado por `ausencia_cobre(..., para='cobranca')`, sem identidade da ausência nesse componente. `app/ponto/services/cartorio.py:1306`, `app/ponto/services/cartorio.py:1348`, `app/ponto/services/cartorio.py:454`. |
| `chamados[].pk` | TAREFA/identidade do chamado | PK de cada chamado selecionado. `app/ponto/services/cartorio.py:455`. |
| `chamados[].status_local` | TAREFA | Estado do chamado. `app/ponto/services/cartorio.py:455`. |
| `dna.dna_versao` | FATO de cadastro/revisão do DNA | Não é a versão da regra do cartório. `app/ponto/services/cartorio.py:456`, `app/ponto/services/cartorio.py:471`. |
| `dna.marcos` | FATO de cadastro usado no julgamento | Representação textual do objeto; não é o DNA inteiro. `app/ponto/services/cartorio.py:456`. |
| `veto` | AVALIAÇÃO | Motivo do veto, quando verdadeiro. `app/ponto/services/cartorio.py:458`. |
| `teto` | AVALIAÇÃO temporal | Quantidade de marcos vencidos passada pelo chamador; não o instante completo. `app/ponto/services/cartorio.py:463`, `app/ponto/services/cartorio.py:1350`. |
| `regra` | AVALIAÇÃO | Versão aplicável à competência. `app/ponto/services/cartorio.py:263`, `app/ponto/services/cartorio.py:471`. |
| `fio` | TAREFA, agregado pelo juiz do fio | Quantidade de chamados vivos com fio vivo; `None` quando não há chamados vivos. `app/ponto/services/cartorio.py:1334`, `app/ponto/services/cartorio.py:1352`. |
| `cob_parcial` | AVALIAÇÃO | Estado do período parcial e índices dos marcos dispensados, derivados da grade. `app/ponto/services/cartorio.py:435`, `app/ponto/services/cartorio.py:508`. |
| `enc` | AVALIAÇÃO temporal | `enc:0` para dia passado ainda em curso; nos demais casos esse componente fica ausente. `app/ponto/services/cartorio.py:481`, `app/ponto/services/cartorio.py:1347`. |
| `isento` | AVALIAÇÃO | `isento:1` somente com isenção e `regra >= 4`. `app/ponto/services/cartorio.py:520`. |

A mistura ocorre literalmente na concatenação: batidas/DNA, cobertura/veto/regra/relógio e chamado/fio produzem o mesmo hash. O próprio `houve_fato_novo` já distingue isso: exclui estado de chamados e fio da definição de fato do dia, porque mudam pelo próprio fechamento. `app/ponto/services/cartorio.py:452`, `app/ponto/services/cartorio.py:329`.

Os consumidores encontrados são:

- **Cartório:** compara impressão atual com `CelulaDia.impressao`, junto de carimbo/veredito, para pular ou rejulgar; `forcar` atravessa o pulo. Há um ramo para nunca-bateu e outro normal. `app/ponto/services/cartorio.py:1199`, `app/ponto/services/cartorio.py:1203`, `app/ponto/services/cartorio.py:1358`, `app/ponto/services/cartorio.py:1362`.
- **Porta `carimbar`:** compara impressão e demais insumos para evitar novo save; depois persiste impressão e carimbos junto da lavra. `app/ponto/portas/celula.py:1047`, `app/ponto/portas/celula.py:1056`, `app/ponto/portas/celula.py:1060`.
- **Releitura após emissão:** usa novamente a impressão para rejulgar células cujo chamado/fio acabou de mudar. Portanto, mudança de tarefa pode disparar novo julgamento sem mudar batida ou DNA. `app/ponto/services/cartorio.py:1015`, `app/ponto/services/cartorio.py:1038`.
- **Comando:** reexporta a função importada; não contém outra implementação. O instrumento `celula_veredito_velho` também lê a impressão para fotografar o estado anterior. `app/ponto/management/commands/processar_cartorio.py:20`, `app/ponto/management/commands/celula_veredito_velho.py:110`.

**Canonicidade: não.** A função não ordena batidas/chamados e usa `str(dna.marcos)`. A carga normal ordena batidas somente por timestamp, sem desempate por PK; chamados usam `-criado_em`, também sem desempate. Inverter as listas ou a inserção das chaves de um `marcos` equivalente altera os bytes serializados — confirmado também por avaliação isolada da função em memória. `app/ponto/services/cartorio.py:453`, `app/ponto/services/cartorio.py:455`, `app/ponto/services/cartorio.py:456`, `app/ponto/turnos.py:2871`, `app/chamados/models.py:100`.

O teste chamado determinístico repete a mesma entrada na mesma ordem. Não achei nele teste de permutação ou de dicionários equivalentes com chaves reordenadas. `app/ponto/tests/test_cartorio.py:14`.

**B — Onde mora o teto e o que depende dele.**

A regra declarada exige **comparar instante ou exigir fato encerrado**; data civil não basta. O selo procura mecanismos no código, mas é uma verificação estrutural por vocabulário, não uma prova completa de todas as combinações temporais. `CLAUDE.md:185`, `app/ponto/tests/test_contract_teto_temporal.py:82`, `app/ponto/tests/test_contract_teto_temporal.py:108`.

Hoje ele aparece em quatro pontos:

1. **Entrada do julgamento:** recusa data futura/célula inexistente; para hoje, sem `teto_de_hoje`, espera o fim do turno. Essa guarda usa `_tz.now()`, enquanto o corpo recebe `agora`. `app/ponto/services/cartorio.py:1974`, `app/ponto/services/cartorio.py:1982`, `app/ponto/services/cartorio.py:1989`.
2. **Vencimento por marco:** `marcos_turno` encadeia os instantes e aplica `TETO_MARCO_MIN = 30`. `app/ponto/services/cartorio.py:828`, `app/ponto/services/cartorio.py:840`.
3. **Conteúdo julgado/lavrado:** marco ainda não vencido sai da grade julgada e sua lâmpada passa a `acesa=None, pendente=True`; não vira falta. `app/ponto/services/cartorio.py:851`, `app/ponto/services/cartorio.py:862`, `app/ponto/services/cartorio.py:1378`.
4. **Atualidade e emissão de perguntas:** `teto`/`enc` alteram a impressão; a fábrica consulta os vencidos para selecionar motivos. `app/ponto/services/cartorio.py:1347`, `app/chamados/services/fabrica_da_celula.py:15`, `app/chamados/services/fabrica_da_celula.py:59`.

O encerramento do turno também considera aberta ainda viva, fim previsto e ausência de previsão; não equivale a “virou o dia”. O teste encontrado mantém a pergunta da saída ausente enquanto o noturno está aberto vivo e a faz nascer após sua expiração. `app/ponto/turnos.py:2114`, `app/ponto/turnos.py:2119`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:351`.

**Consequência para o desenho:** por inferência dessas dependências, o instante de referência, os marcos vencidos/pendentes, a condição de turno encerrado e a política temporal precisam compor a **versão da avaliação**. Se restar apenas a versão dos fatos, o relógio avança sem necessariamente mudar batidas/DNA, e o pulo pode preservar avaliação vencida. O teste cartório 06:27 → vigia 06:40 cobre justamente a mudança temporal que precisa provocar nova pergunta. `app/ponto/services/cartorio.py:1358`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:361`.

Não confundir esse teto com **horizonte de regra** ou **piso de emissão**: a versão é escolhida por competência; o piso limita emissão automática e deliberadamente fica fora da impressão. `app/ponto/services/cartorio.py:263`, `app/ponto/services/cartorio.py:270`, `app/ponto/services/cartorio.py:285`, `app/ponto/services/cartorio.py:1676`.

**C — Identidade real e concorrência.**

| Objeto | Chave/agrupamento efetivo |
|---|---|
| **Pergunta** | `(colaborador da disputa, motivo, data_evento)`, entre todas as disputas/estados; reutiliza a última encontrada. Sem data na chamada, trata como conversa e nasce sempre. `app/chamados/models.py:2392`, `app/chamados/models.py:2398`, `app/chamados/models.py:2424`, `app/chamados/models.py:2431`. |
| **Disputa** | Procura aberta por colaborador, agrega motivos e contexto. Não representa uma tarefa individual por marco. `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:472`, `app/chamados/services/disputa_emissao.py:498`. |
| **Chamado** | `abrir` deduplica pelos filtros recebidos. No emissor de ausência: colaborador + módulo + `contexto_json.data_turno`. O pai de disputa usa colaborador + categoria + módulo + estados vivos. `app/chamados/models.py:867`, `app/ponto/management/commands/detectar_ausencias.py:693`, `app/chamados/services/disputa_emissao.py:504`. |

**O exemplo intervalo versus saída final não se funde necessariamente:** `intervalo_saida` e `orfao_14h` são motivos distintos. Entretanto, dois marcos com o mesmo motivo e data atingem a mesma chave; hora, posição e instância da tarefa não a integram. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:2392`.

A data também não é uniformemente jornada: o emissor de intervalo pode usar o dia civil do marco, e seu teste noturno espera D+1 nas perguntas. `app/chamados/services/disputa_emissao.py:982`, `app/chamados/services/disputa_emissao.py:991`, `app/chamados/tests/test_fio_intervalo.py:77`.

**Banco:** não achei índice único parcial de tarefa aberta nos três modelos nem nas migrations de chamados consultadas. Chamado tem `unique_together=[]`; disputa tem índice comum `(colaborador, fechada_em)`; pergunta tem apenas ordenação no `Meta`. O unique parcial encontrado é de **Followup.evento_chave**, não da identidade da obrigação. `app/chamados/models.py:97`, `app/chamados/models.py:1742`, `app/chamados/models.py:2857`, `app/chamados/migrations/0047_evento_chave_do_fio.py:20`.

**Há `select_for_update`:**

- Abertura de chamado trava o colaborador antes do dedup; transição protegida recarrega/trava o chamado; costura trava o destino quando está em transação. `app/chamados/models.py:863`, `app/chamados/models.py:668`, `app/chamados/services/fio.py:758`.
- Emissão de disputa trava a disputa aberta **existente**. A consulta vazia não oferece linha para travar. `app/chamados/services/disputa_emissao.py:466`.

**Corridas possíveis por inspeção, sem ocorrência de produção demonstrada:**

- Pergunta: consulta `.last()` → `create`, sem lock no próprio gate. `app/chamados/models.py:2431`, `app/chamados/models.py:2476`.
- Disputa nova: consulta vazia → abre/reusa pai → cria disputa, sem repetir a consulta após a trava do colaborador. `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:523`.
- Validação: lê/testa `validada_em` antes do `atomic`, sem travar a pergunta nesse fluxo. `app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:366`, `app/chamados/services/validacao.py:395`.
- Fechamento de disputa: confere estado/perguntas antes do `atomic` e depois grava, sem releitura travada nesse método. `app/chamados/models.py:1820`, `app/chamados/models.py:1827`, `app/chamados/models.py:1846`, `app/chamados/models.py:1861`.

**D — O que fecha, recusa e preservação da absorção.**

Para a **pergunta**, o primeiro ramo do juiz considera `validada_em` suficiente para deixá-la morta; `estado` distingue `validada` de `resolvida` pela presença adicional de `materializada_em`. Portanto, não existe hoje uma exigência universal de “efeito efetivado + rejulgamento consistente”. `app/chamados/juizes.py:1135`, `app/chamados/models.py:2499`.

O plantio normal de HORA/data-hora tem defesa adicional: executa em transação, exige sucesso e `_pergunta_materializada_de_fato`, desfaz a batida se reprovada e só então carimba. Contudo, consultar C6 após `carimbar(resposta='validar')` não é prova independente: esse carimbo pode preencher validação antes da consulta ao juiz que mata pela validação. `app/chamados/services/materializacao.py:1195`, `app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1252`, `app/chamados/models.py:2591`, `app/chamados/models.py:2596`.

Para a **disputa**, o fechamento normal exige ausência de respostas sem veredito e de fio vivo. O materializador chama esse juiz; não fecha simplesmente pelo número de batidas criadas. Há portas excepcionais, inclusive encerramento pelo admin/desligamento. `app/chamados/models.py:1783`, `app/chamados/models.py:1789`, `app/chamados/models.py:1852`, `app/chamados/services/materializacao.py:1288`.

Para o **chamado**, o materializador passa pela retenção de irmãs e por `reconciliar()`; fechar a disputa também aciona o tratamento do pai. Não são o mesmo estado nem uma única condição de fechamento. `app/chamados/services/materializacao.py:736`, `app/chamados/services/materializacao.py:746`, `app/chamados/models.py:1812`.

**Validada com materialização recusada tem respostas diferentes:**

- **HORA declarada, sem materialização e com recusa registrada:** desfaz `validada_em/por`, preserva resposta, reabre a disputa por padrão e retorna `nao_gravou/devolvida`. Fica respondida e viva para o **admin**. O teste exige exatamente isso. `app/chamados/services/validacao.py:180`, `app/chamados/services/validacao.py:187`, `app/chamados/services/validacao.py:199`, `app/chamados/services/validacao.py:460`, `app/chamados/tests/test_o261_recusa_fala.py:95`.
- **Escape, outro tipo ou encerramento já carimbado por identidade:** essa devolução não se aplica; a validação pode permanecer e C6 a considera morta. Os testes preservam explicitamente os casos de escape e colisão já carimbada. `app/chamados/services/validacao.py:143`, `app/chamados/services/validacao.py:182`, `app/chamados/tests/test_o261_recusa_fala.py:190`, `app/chamados/tests/test_o261_recusa_fala.py:203`.
- **Alcançabilidade:** resposta não validada atribui ação ao admin, mas o crédito depende do destino/pai vivo e a lista padrão exclui arquivado/histórico. Logo, “viva” sozinha não prova porta alcançável. `app/chamados/catalogo/natureza.py:48`, `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:60`, `app/chamados/services/verbos.py:18`.

Há uma divergência relevante entre comentário e código: o comentário da devolução de ausência diz que o app só lista disputa aberta; o seletor atual de perguntas do app consulta `q_vivas()` e depois o juiz, sem aquele filtro explícito. Não usaria o comentário como prova da seleção atual. `app/chamados/services/materializacao.py:632`, `app/chamados/services/pergunta_no_app.py:42`, `app/chamados/models.py:2793`.

**Compatibilidade com pós-condição:** separar conclusão da tarefa do registro do efeito é compatível como direção, mas não basta mudar o predicado de fechamento. Hoje `materializada_em` identifica também o **ato de absorção** e encerramentos sem batida, inclusive paridade recusada. `app/chamados/services/materializacao.py:433`, `app/chamados/services/materializacao.py:585`.

A recuperação de absorvidas depende de:

- `(disputa_id, data_evento, materializada_em)` compartilhado pelas irmãs;
- escape correspondente ao tipo da ausência dentro desse grupo;
- exclusão das perguntas que já possuem batida viva originada nelas. `app/chamados/services/materializacao.py:605`, `app/chamados/services/materializacao.py:609`, `app/chamados/services/materializacao.py:614`, `app/chamados/services/materializacao.py:616`.

**Quebraria essa recuperação**, por consequência direta dos filtros: deixar `materializada_em` nulo até a aprovação final do atestado, dar timestamps distintos às irmãs ou mover apenas parte delas para outra disputa. Hoje a absorção usa o mesmo instante mesmo com ausência aberta; nessa fase o chamado permanece vivo/silenciado. Seria necessário preservar esse identificador de ato ou substituí-lo com ligação equivalente antes de retirar a autoridade desses campos. `app/chamados/services/materializacao.py:529`, `app/chamados/services/materializacao.py:549`, `app/chamados/services/materializacao.py:560`, `app/chamados/services/materializacao.py:607`.

**E — Dez casos de laboratório, condicionados aos respectivos insumos.**

| Caso | Comportamento hoje / quem age | Cobertura encontrada |
|---|---|---|
| **1. Dois marcos no dia** | Motivos diferentes podem gerar perguntas distintas sob a mesma disputa/chamado; silêncio pede resposta do colaborador, resposta não validada pede admin. Resolver uma pergunta não basta para fechar disputa com outra viva. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/catalogo/natureza.py:48`, `app/chamados/models.py:1789`. | Dois marcos de intervalo: `app/chamados/tests/test_fio_intervalo.py:62`. **Não achei o caso exato intervalo-saída + saída-final**, nas buscas em `app/chamados/tests` e `app/ponto/tests`; cobertura específica `[nao verificado]`. |
| **2. Noturno 21→22, sem 23** | O plantio toma `data_evento`, combina a hora e ajusta usando os marcos dessa data, tentando D e D+1. Como uma fábrica já pode gravar D+1, **não está demonstrada a garantia contra deslocar novamente**. Recusa de HORA volta ao admin pelo fluxo descrito em D. `app/chamados/services/materializacao.py:344`, `app/chamados/services/materializacao.py:414`, `app/chamados/services/materializacao.py:420`, `app/chamados/juizes.py:303`, `app/chamados/juizes.py:310`. | Ajuste isolado para D+1 e entrada sem deslocamento: `app/chamados/tests/test_plantio_intervalo_overnight.py:27`, `app/chamados/tests/test_plantio_intervalo_overnight.py:35`; reuso em D+1: `app/chamados/tests/test_materializar_overnight.py:33`. **Não achei teste integrado 21→22 sem 23** nesses testes e nas buscas de noturno; `[nao verificado]`. |
| **3. Validada/materialização recusada** | HORA sem carimbo e com motivo: validação desfeita, resposta preservada, admin recebe novamente; não exige nova resposta do colaborador. Fora desse escopo, pode permanecer validada/morta. `app/chamados/services/validacao.py:182`, `app/chamados/services/validacao.py:187`, `app/chamados/services/validacao.py:460`, `app/chamados/juizes.py:1135`. | Devolução, irmãs preservadas e exceções: `app/chamados/tests/test_o261_

[truncado em 20000 de 26519 caracteres]

## 06/11 21:30 · sessao 62c88a377430 · DUAS-TELAS-E-PWA-OFFLINE · censo · origem=Code

**Censo estático: 40 views POST examinadas; 39 com escrita, sendo 32 no recorte C. Normalizei 25 famílias de ação; 12 têm múltiplas telas comprovadas. Resultado: 23 pares divergentes, discriminados abaixo.** A contagem considera funções distintas, sem duplicar aliases de rota; apresentações equivalentes ficam agrupadas. As referências do inventário sustentam essa contagem. Ocorrência, frequência e valores afetados em produção: **[não verificado]**.

A referência adotada foi célula como fonte e projeções como leitores; escritor único conforme o glossário e autoridades do código vivo. (`app/docs/GLOSSARIO.md:11`; `app/docs/GLOSSARIO.md:12`; `app/docs/PLANO-PADROES.md:11`; `app/core/juizes.py:288`)

| Universo examinado | Views POST com escrita | Evidência |
|---|---:|---|
| Escala | 9 | Remover vínculo, editar calendário, aplicar calendário, corroborar e realocar régua; salvar tipo, criar calendário pelo wizard; toggle e lote de folgas. (`app/escala/views.py:256`; `app/escala/views.py:316`; `app/escala/views.py:376`; `app/escala/views.py:456`; `app/escala/views.py:463`; `app/escala/views_wizard.py:98`; `app/escala/views_wizard.py:140`; `app/escala/views_plano_folgas.py:98`; `app/escala/views_plano_folgas.py:144`) |
| Folha | 5; 1 no recorte | TXT, rubricas e três operações de integração. (`app/folha/views.py:102`; `app/folha/views.py:27`; `app/folha/views.py:466`; `app/folha/views.py:487`; `app/folha/views.py:501`) |
| Relatórios | 3 | Espelho anexado, avulso e lote. (`app/relatorios/views.py:971`; `app/relatorios/views.py:1050`; `app/relatorios/views.py:1152`) |
| Holerite | 6; 3 no recorte | Três uploads/publicação; enviar, reenviar e avisar ficaram fora da comparação de entrega. (`app/holerite/views.py:1145`; `app/holerite/views.py:1217`; `app/holerite/views.py:1721`; `app/holerite/views.py:605`; `app/holerite/views.py:734`; `app/holerite/views.py:1794`) |
| Fronteiras dos mesmos domínios | 16 | Sete em ponto, três em feriados, duas em vínculos e quatro portas de aceite de folga. (`app/ponto/views.py:1226`; `app/ponto/views.py:1257`; `app/ponto/views.py:1383`; `app/ponto/views.py:1434`; `app/ponto/views.py:1461`; `app/ponto/views.py:1520`; `app/ponto/views.py:3312`; `app/core/views_config.py:39`; `app/core/views_config.py:153`; `app/core/views_config.py:182`; `app/colaboradores/views.py:212`; `app/colaboradores/views.py:373`; `app/chamados/views.py:359`; `app/chamados/views.py:659`; `app/chamados/views.py:869`; `app/chamados/views_cobrar.py:145`) |

O quadragésimo POST é o preview do wizard, sem persistência. Copiar/excluir tipo também foram examinados: escrevem por GET e não entram no total POST. (`app/escala/views_wizard.py:226`; `app/escala/services/cadastro_tipo.py:396`; `app/escala/views.py:141`; `app/escala/views.py:156`; `app/templates/escala/tipos_lista.html:90`; `app/templates/escala/tipos_lista.html:92`)

As 25 famílias são: treze de escala/folga — criar, editar, copiar e excluir tipo; encerrar e vincular/realocar vínculo; corroborar régua; criar, editar e atribuir calendário; lançar, alterar e remover folga —, cinco de fechamento/célula, TXT, publicação e análise de holerite, espelho PDF e três de feriado. (`app/escala/urls.py:8`; `app/escala/urls.py:15`; `app/escala/urls.py:19`; `app/escala/urls.py:31`; `app/escala/urls.py:34`; `app/ponto/urls.py:31`; `app/ponto/urls.py:58`; `app/folha/urls.py:14`; `app/holerite/urls.py:8`; `app/holerite/urls.py:37`; `app/relatorios/urls.py:24`; `app/core/urls.py:42`)

**1. Criar calendário de folgas — 1 par divergente.**

| Tela A → escritor | Tela B → escritor |
|---|---|
| `/escala/folgas/criar/` → `editar_folga_calendario`, `app/escala/views.py:316`; template `app/templates/escala/folga_calendario_editar.html:9` → **`salvar_calendario_de_folga`**, `app/escala/folgas.py:548`. | Wizard `/escala/tipos/wizard/`, botão em `app/templates/escala/wizard_tipo_escala.html:160`; POST `/escala/tipos/wizard/calendario/criar/` → `wizard_calendario_criar`, `app/escala/views_wizard.py:140` → **`criar_calendario_de_folga`**, `app/escala/folgas.py:577`. |

**Difere:** editor recebe listas independentes para meses de 28/30/31 dias; wizard replica uma lista com descarte dos dias acima do tamanho. Pares semana/dia malformados são ignorados pelo editor e recusados pelo wizard. Convergem apenas no veto de nome repetido. (`app/escala/views.py:333`; `app/escala/views.py:340`; `app/escala/views_wizard.py:163`; `app/escala/folgas.py:539`; `app/escala/folgas.py:570`; `app/escala/folgas.py:590`)

**Leitor / gravidade:** `datas_que_o_calendario_manda` alimenta `gerar_foto_folgas`, que grava `FolgaDia` e relavra células. **Muda dia: sim, potencialmente após materialização; o cadastro isolado só grava o calendário.** (`app/escala/folgas.py:43`; `app/escala/folgas.py:150`; `app/escala/folgas.py:184`; `app/escala/folgas.py:203`)

**2. Editar folga semanal do tipo — 1 par divergente, convergindo no escritor.**

| Tela A → escritor | Tela B → escritor |
|---|---|
| `/escala/tipos/<tid>/wizard/`; POST `/escala/tipos/<tid>/wizard/salvar/` → `wizard_salvar`, `app/escala/views_wizard.py:98`; template `app/templates/escala/wizard_tipo_escala.html:128` → **`salvar_tipo`**, `app/escala/services/cadastro_tipo.py:140`. | Modal `/chamados/modal/<colaborador>/`; template `app/templates/chamados/partials/modal_fio.html:545` envia `aceitar_folga_limbo` para `/chamados/local/<id>/` → `detalhe_chamado_local`, `app/chamados/views.py:431` → `aceitar_sugestao`, `app/chamados/services/limbo_folgas.py:73` → **mesmo `salvar_tipo`**, chamado em `app/chamados/services/limbo_folgas.py:91`. |

**Difere:** wizard recebe os dias escolhidos; aceite exige sugestão fixa do contexto. Só o aceite acrescenta `LogAuditoria` específico e percorre chamados LIMBO do tipo, reconciliando e anexando ocorrência quando o fechamento passa. (`app/escala/views_wizard.py:44`; `app/chamados/services/limbo_folgas.py:81`; `app/chamados/services/limbo_folgas.py:94`; `app/chamados/services/limbo_folgas.py:98`)

**Leitor / gravidade:** campo e regeneração são comuns; o fio lê as ocorrências adicionais. **Dinheiro/dia: não há diferença comprovada para os mesmos dias; trilha e chamados diferem.** (`app/escala/services/cadastro_tipo.py:259`; `app/escala/services/cadastro_tipo.py:320`; `app/chamados/views.py:372`)

**3. Confirmar cadastro de escala errado — 1 par divergente, convergindo no escritor.**

| Tela A → escritor | Tela B → escritor |
|---|---|
| `/escala/cadastro-x-realidade/`; template `app/templates/escala/cadastro_x_realidade.html:28`; POST `.../regua/<ec_pk>/corroborar/` → `corroborar_regua_da_lista`, `app/escala/views.py:456` → **`corroborar_regua`**, `app/escala/regua_defesa.py:647`. | Modal `/chamados/modal/<colaborador>/`; template `app/templates/chamados/partials/modal_fio.html:559`; POST `/chamados/local/<id>/`, ação `corroborar_regua` → `app/chamados/views.py:438` → **mesmo `corroborar_regua`**. |

**Difere:** lista endereça o vínculo pela PK; chamado procura vínculo ativo por pessoa e tipo guardado no contexto. Só o caminho do chamado passa `chamado=` e anexa ocorrência. Ambos gravam régua pela mesma porta. (`app/escala/views.py:449`; `app/chamados/views.py:441`; `app/chamados/views.py:443`; `app/escala/regua_defesa.py:662`; `app/escala/regua_defesa.py:670`)

**Leitor / gravidade:** cobrança lê a régua; fio lê a ocorrência. **Dinheiro/dia: não para o mesmo vínculo; alvo diferente entre as telas é possível, mas sua ocorrência em produção está [não verificado].** (`app/escala/regua_defesa.py:641`; `app/escala/regua_defesa.py:662`; `app/chamados/views.py:372`)

**4. Vincular/realocar escala — 3 pares divergentes, convergindo nas portas.**

| Caminho | Tela, view, template → escritor |
|---|---|
| A — manual | `/colaboradores/vincular/<cid>/painel/`; POST `/colaboradores/<cid>/vincular/escala/` → `vincular_escala`, `app/colaboradores/views.py:373`; template `app/templates/colaboradores/partials/painel_vinculo.html:136` → **`julgar_vinculo` / `executar_vinculo`**, chamadas em `app/colaboradores/views.py:411` e `app/colaboradores/views.py:423`. |
| B — lista sugerida | `/escala/cadastro-x-realidade/`; POST `.../regua/<ec_pk>/realocar/` → `realocar_regua_da_lista`, `app/escala/views.py:463`; template `app/templates/escala/cadastro_x_realidade.html:28` → `realocar_vinculo_para_destino`, `app/escala/regua_defesa.py:291` → **mesmas portas**, `app/escala/regua_defesa.py:316`. |
| C — chamado sugerido | Modal `/chamados/modal/<colaborador>/`; template `app/templates/chamados/partials/modal_fio.html:568`; POST `/chamados/local/<id>/`, ação `realocar_regua` → `app/chamados/views.py:449` → `realocar_para_destino`, `app/escala/regua_defesa.py:326` → **mesmas portas**, `app/escala/regua_defesa.py:329`. |

- **A×B e A×C:** manual recebe destino, início e âncora humanos; sugeridos fixam início em hoje e recusam ciclos antes do juiz. Manual pode alcançar a janela anterior e criar ciclo com âncora declarada. (`app/templates/colaboradores/partials/painel_vinculo.html:155`; `app/templates/colaboradores/partials/painel_vinculo.html:159`; `app/templates/colaboradores/partials/painel_vinculo.html:167`; `app/escala/regua_defesa.py:310`; `app/escala/regua_defesa.py:313`; `app/colaboradores/services/vinculo.py:69`; `app/colaboradores/services/vinculo.py:126`)
- **B×C:** lista usa destino da última lavra; chamado usa destino do seu contexto. Só C acrescenta ocorrência de realocação. (`app/escala/views.py:469`; `app/escala/regua_defesa.py:328`; `app/escala/regua_defesa.py:333`)

**Leitor / gravidade:** `eh_dia_trabalho` lê a célula; executar vínculo regenera o trecho escolhido, e a porta recalcula competências tocadas. **Muda dia/dinheiro: sim, potencialmente, pelo destino e período distintos.** Igualdade dos destinos em produção: **[não verificado]**. (`app/escala/models.py:1094`; `app/colaboradores/services/vinculo.py:320`; `app/ponto/portas/celula.py:112`; `app/ponto/portas/celula.py:445`; `app/ponto/portas/celula.py:458`)

**5. Lançar folga manual/aceitar “era folga” — 6 pares divergentes.**

| Caminho | Tela, view, template → escritor |
|---|---|
| P — Plano | `/escala/plano-folgas/`; POST `/escala/plano-folgas/aplicar/` → `app/escala/views_plano_folgas.py:144`; template `app/templates/escala/plano_folgas.html:171` → **`aplicar_folgas_em_lote`**, `app/escala/folgas.py:362` → `_foto_do_dia`, `app/escala/folgas.py:320`. |
| D — detalhe | `/chamados/local/<id>/` → `app/chamados/views.py:359`, ramo em `app/chamados/views.py:483`; template `app/templates/chamados/detalhe_local.html:310`, formulário `app/templates/chamados/partials/_pergunta_card.html:92` → **`validar_pergunta`**, `app/chamados/services/validacao.py:382`. |
| M — modal | `/chamados/modal/<colaborador>/` → `app/chamados/views.py:869`, delegação em `app/chamados/views.py:954`; template `app/templates/chamados/partials/modal_fio.html:274` → **mesmo `validar_pergunta`**, por `app/chamados/services/fio.py:605`. |
| I — inline | Painel `/chamados/painel/`; botão `app/templates/chamados/partials/_lista_chamados.html:82`; POST `/chamados/validar-inline/<cid>/` → `app/chamados/views_cobrar.py:145` → **mesmo `validar_pergunta`**, chamada em `app/chamados/views_cobrar.py:208`. |

Nos aceites de folga, a cadeia termina em `declarar_dia` → **`lancar_folga`**; o Plano usa outro escritor da foto. (`app/chamados/services/validacao.py:291`; `app/ponto/services/declarar_dia.py:189`; `app/escala/folgas.py:280`; `app/escala/folgas.py:337`)

| Pares | Diferença |
|---|---|
| P×D, P×M, P×I | Plano recusa dia anterior à competência atual; aceite usa teto de antiguidade de 120 dias. Plano recebe F1…F9/FD; aceite fixa FD. Aceite carimba pergunta, fecha perguntas e tenta reconciliar chamados; Plano solicita relavra sem passar por esse encerramento explícito. (`app/escala/folgas.py:379`; `app/escala/folgas.py:385`; `app/escala/views_plano_folgas.py:173`; `app/ponto/services/declarar_dia.py:101`; `app/ponto/services/declarar_dia.py:190`; `app/ponto/services/declarar_dia.py:204`; `app/chamados/services/validacao.py:299`; `app/chamados/services/validacao.py:300`) |
| D×M, I×M | Detalhe e inline exigem disputa aberta; modal admite pergunta viva em disputa fechada e acrescenta trilha desse ato. **Convergem no escritor, divergem na validação anterior.** (`app/chamados/views.py:495`; `app/chamados/views_cobrar.py:160`; `app/chamados/services/fio.py:596`; `app/chamados/services/fio.py:601`; `app/chamados/services/fio.py:611`) |
| D×I | Detalhe valida a pergunta endereçada; inline percorre todas as perguntas vivas respondidas da disputa encontrada. **Convergem no escritor, diferem na seleção do ato.** (`app/chamados/views.py:493`; `app/chamados/views.py:501`; `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:201`) |

**Leitor / gravidade:** precedência lê a folga; relavra chega à célula. **Muda dia: sim, potencialmente**, sobretudo no passado que P recusa e os aceites podem alcançar. Dinheiro efetivamente alterado: **[não verificado]**. (`app/ponto/precedencia.py:356`; `app/ponto/precedencia.py:384`; `app/escala/folgas.py:272`)

**6. Aprovar fechamento — 3 pares divergentes.**

| Caminho | Tela, view, template → escritor |
|---|---|
| A — competência/selecionados | `/ponto/fechamento/`; POST `/ponto/fechamento/aprovar/` → `app/ponto/views.py:1257`; disparador `app/templates/ponto/fechamento.html:143` → **`aprovar_lote`**, `app/ponto/services/fechamento.py:1316`. |
| B — individual | `/ponto/fechamento/painel/` → `app/ponto/views.py:966`; template `app/templates/ponto/partials/painel_fechamento.html:86`; POST `/ponto/fechamento/aprovar-colaborador/` → `app/ponto/views.py:1461` → **`aprovar`**, `app/ponto/services/fechamento.py:1328`. |
| C — ao gerar TXT | `/folha/`, template `app/templates/folha/index.html:63`, ou `/folha/exportar/`, template `app/templates/folha/exportar.html:26`; POST `/folha/exportar/gerar/` → `app/folha/views.py:102` → **mesmo `aprovar_lote` de A**, chamado em `app/folha/views.py:252`. |

- **A×B:** A fornece linhas gravadas ao juiz; B fornece `fechamento_lido`. A grava observação por update; B faz save inteiro. A pode trancar o período; B apenas aprova. (`app/ponto/views.py:1290`; `app/ponto/views.py:1480`; `app/ponto/views.py:1362`; `app/ponto/views.py:1367`; `app/ponto/services/fechamento.py:1333`)
- **A×C:** convergem em `aprovar_lote`; C calcula em leitura, carimba apenas elegíveis para o TXT, mantém observação, registra auditoria própria e exportação. A pode trancar; C não. (`app/folha/views.py:208`; `app/folha/views.py:246`; `app/folha/views.py:252`; `app/folha/views.py:254`; `app/folha/views.py:297`; `app/ponto/services/fechamento.py:1323`)
- **B×C:** leitura viva em ambos, mas escritores/save diferentes e universo de aprovação diferente; C acrescenta TXT e sua trilha. (`app/ponto/views.py:1480`; `app/ponto/views.py:1515`; `app/folha/views.py:246`; `app/folha/views.py:252`; `app/folha/views.py:297`)

O modo cego de A considera conta vazia; selecionados/B dispensam essa trava. Seu formulário existe **dentro de `display:none`**, portanto não presumi botão visível. (`app/ponto/views.py:1304`; `app/ponto/views.py:1509`; `app/templates/ponto/fechamento.html:152`; `app/templates/ponto/fechamento.html:209`)

**Leitor / gravidade:** tranca aciona encerramento da fila; status aprovado é lido por `competencia_fechada`, podendo impedir ajuste posterior. **Dinheiro/dia: não diretamente no carimbo; muda controle de escrita e fila.** A regra “TXT não tranca” está declarada como intencional no registro vivo. (`app/ponto/services/fechamento.py:1322`; `app/ponto/services/fechamento.py:1413`; `app/ponto/janelas.py:87`; `app/ponto/services/flip_batida.py:21`; `app/core/juizes.py:510`)

**7. Publicar/substituir holerite da competência — 3 pares divergentes.**

| Caminho | Tela, view, template → escritor |
|---|---|
| A — avulso | `/holerite/upload/` → `app/holerite/views.py:1145`; template `app/templates/holerite/upload.html:9` → **delete/create na view**, `app/holerite/views.py:1191`, `app/holerite/views.py:1193`. |
| B — ZIP | `/holerite/upload-lote/` → `app/holerite/views.py:1217`; template `app/templates/holerite/upload_lote.html:12` → **delete/create na view**, `app/holerite/views.py:1255`, `app/holerite/views.py:1259`. |
| C — PDF processado | `/holerite/lote/` → `app/holerite/views.py:1721`; confirmação `app/templates/holerite/_lote_progresso.html:119` → job `app/holerite/management/commands/processar_lote_holerite.py:89` → **`motor._gravar`**, `app/holerite/motor.py:390`, documento em `app/holerite/motor.py:480`. |

| Par | Diferença e gravidade |
|---|---|
| A×B | A recusa substituir documento com ciência e audita substituição; B apaga sem essa guarda. A preenche empresa da folha; B omite. A assinatura possui CASCADE. **Dinheiro/dia: não diretamente; pode apagar ciência e mudar a identificação na matriz.** (`app/holerite/views.py:1176`; `app/holerite/views.py:1184`; `app/holerite/views.py:1202`; `app/holerite/views.py:1255`; `app/holerite/views.py:1259`; `app/holerite/models.py:193`; `app/holerite/views.py:295`) |
| A×C | A substitui documentos sem ciência e cria sem lote; C pula quem já tem documento, grava empresa do CNPJ e vínculo ao lote publicado. **Muda dia/dinheiro: sim, potencialmente pela guarda da competência**, além da substituição diferente. (`app/holerite/views.py:1191`; `app/holerite/views.py:1193`; `app/holerite/motor.py:323`; `app/holerite/motor.py:344`; `app/holerite/motor.py:479`; `app/holerite/management/commands/processar_lote_holerite.py:129`) |
| B×C | B apaga e recria sem empresa/lote; C publica incrementalmente, preservando documentos existentes e vinculando ao lote. **Muda dia/dinheiro: sim, potencialmente pela mesma guarda; também há diferença de preservação da ciência.** (`app/holerite/views.py:1255`; `app/holerite/views.py:1259`; `app/holerite/motor.py:344`; `app/holerite/motor.py:480`; `app/holerite/models.py:193`) |

**Leitor que torna essa diferença relevante:** `competencia_fechada_para_celula` só considera holerite com `lote.publicado_em` preenchido. A guarda de regeneração enumera esses documentos; edição do dia também consulta essa proteção. Assim, “publicado” pelos uploads A/B não produz a mesma proteção de C. Ocorrência concreta em produção: **[não verificado]**. (`app/folha/export.py:614`; `app/ponto/management/commands/gerar_celulas.py:169`; `app/ponto/portas/celula.py:568`)

**8. Analisar lote de holerite — 1 par divergente, convergindo no escritor.**

A página `/holerite/lote/`, `upload_lote_v2`, template `app/templates/holerite/upload_lote_v2.html:23`, e o drawer `/holerite/celula/<empresa>/<ano>/<mes>/modo/?modo=lote`, `drawer_modo`, template `app/templates/holerite/_drawer_modo_lote.html:13`, POSTam para a mesma view e **convergem na criação de `LoteHolerite` e no job**. (`app/holerite/views.py:340`; `app/holerite/views.py:357`; `app/holerite/views.py:1721`; `app/holerite/views.py:1773`)

**Difere:** drawer informa empresa esperada; página genérica não. O job verifica divergência de CNPJ na análise quando esse contexto existe. **Dinheiro/dia: não neste ato; muda a validação que antecede a publicação e o estado/erro mostrado no lote.** (`app/templates/holerite/_drawer_modo_lote.html:19`; `app/holerite/views.py:1771`; `app/holerite/views.py:1779`; `app/holerite/management/commands/processar_lote_holerite.py:107`; `app/holerite/management/commands/processar_lote_holerite.py:137`)

**9. Emitir/exportar espelho PDF — 3 pares divergentes.**

| Caminho | Tela, view, template → escritor |
|---|---|
| A — anexar | `/chamados/local/<id>/`, formulário `app/templates/chamados/detalhe_local.html:348`; POST `/relatorios/espelho/chamado/<id>/anexar/` → `app/relatorios/views.py:971` → **`gerar_pdf_espelho`**, `app/relatorios/services.py:49`. |
| B — avulso | `/relatorios/espelho/avulso/` → `app/relatorios/views.py:1050`; template `app/templates/relatorios/espelho_pdf_avulso.html:18` → **mesmo `gerar_pdf_espelho`**, chamada em `app/relatorios/views.py:1127`. |
| C — lote 

[truncado em 20000 de 26787 caracteres]

## 06/11 21:28 · sessao 3ef57447b4ee · - · - · origem=Ronald (tmux)

E — achados de leitura; testes encontrados, não executados. [nao verificado]

| Caso | Comportamento demonstrado hoje | Testes/limite | [nao verificado]
|---|---|---|
| **1. Dois marcos no dia** | Há perguntas distintas para motivos distintos; o dedup datado usa `(disputa,motivo,data_evento)`, portanto `intervalo_saida` e outro motivo não necessariamente se fundem. Isso ainda não é identidade explícita por MARCO/instância: `app/chamados/services/disputa_emissao.py:368`, `app/chamados/services/disputa_emissao.py:387`, `app/chamados/services/disputa_emissao.py:389`. | Dois marcos de intervalo no mesmo chamado geram duas perguntas: `app/chamados/tests/test_fio_intervalo.py:62`. **Não achei teste exato “falta saída intervalo + falta saída final”** nas buscas por marcos/motivos em `app/chamados/tests` e `app/ponto/tests` [nao verificado]. |
| **2. Noturno 21→22 sem 23** | O plantio parte de `p.data_evento`, combina esse dia com a hora e depois ajusta overnight: `app/chamados/services/materializacao.py:344`, `app/chamados/services/materializacao.py:414`, `app/chamados/services/materializacao.py:420`. O ajuste consulta a escala desse dia e tenta somente D e D+1 dentro do envelope dos marcos: `app/chamados/juizes.py:302`, `app/chamados/juizes.py:310`. **Não há garantia demonstrada contra revirada quando `data_evento` já contém o dia civil do marco**: o teste legado grava D+1 nas perguntas de intervalo (`app/chamados/tests/test_fio_intervalo.py:77`), enquanto o ajuste toma esse dia como base (`app/chamados/juizes.py:303`). | Próximos: volta 00:00 vai a D+1, entrada 19:00 não desloca: `app/chamados/tests/test_plantio_intervalo_overnight.py:27`, `app/chamados/tests/test_plantio_intervalo_overnight.py:35`; saída existente D+1 é reutilizada: `app/chamados/tests/test_materializar_overnight.py:33`. **Não achei teste exato 21→22 sem 23** nesses arquivos e na busca de datas/noturno em `app/chamados/tests` [nao verificado]. |
| **4. Atestado pendente/aprovado/rejeitado** | Escape ATESTADO cria ausência com estado decidido por `estado_inicial`; sem documento, `aguardando_documento`. A absorção tenta carimbar perguntas-hora do mesmo dia, mas o C6 pode recusar e deixá-las vivas; ausência aberta mantém chamado vivo e o silencia: `app/chamados/services/materializacao.py:495`, `app/chamados/services/materializacao.py:531`, `app/chamados/services/materializacao.py:549`, `app/chamados/services/materializacao.py:562`. Rejeição devolve perguntas absorvidas antes de levantar silêncio: `app/ponto/signals.py:254`. | Rejeitada: perguntas pendentes/vivas e disputa aberta, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88`. Aprovada não devolve, `:129`. Outra ausência viva segura absorção, `:119`; pergunta com batida não volta, `:141`; nova resposta reabre a MESMA ausência, `:154`; reabertura só pelo DP não reabsorve, `:172`. |
| **5. Troca de plantão nos dois colabs** | Exige par de ausências/cartas, pessoas distintas e documentos nos dois lados. Trava cartas, edita os dois dias no mesmo atomic; cedente vira folga, substituto dia normal pelo template próprio; recusa do segundo lado desfaz primeiro: `app/ponto/portas/celula.py:632`, `:652`, `:657`, `:662`, `:673`. A mudança marca cobranças órfãs para rejulgamento e agenda juiz antes do recalculo, por competência dos dias: `app/ponto/portas/celula.py:622`, `:627`, `:422`, `:445`. Falta em folga tocada é reavaliada no pós-commit: `app/ponto/portas/celula.py:490`. | Cedente sem desconto `app/ponto/tests/test_o10b_editar_dia_troca.py:159`; substituto sem HE100/folga trabalhada e mantendo noturno `:169`; competências distintas `:260`; falha entre lados rollback `:325`; idempotência `:238`; sem par/documentos/vínculo recusa `:213`, `:222`, `:350`. O teste de concorrência existente verifica AST/locks, não duas transações reais: `:314`. |
| **8. Passivo sem inventar** | O plantio recusa texto vazio e grava recusa com dono; não toma validação/admin ou carimbo de resposta como anuência: `app/chamados/services/materializacao.py:804`. Lote de passivo usa mesma unidade oficial, classificação do vigia; exclui slot não derivável e plantio já fora do slot: `app/chamados/services/passivo_validado.py:39`, `:60`. O dry não chama unidade: `:360`; aplica por `(colab,competência)` atomic e fecha por disputa uma vez ao final: `:371`, `:403`. | Recusa sem resposta: `app/chamados/tests/test_a27_recusa_com_dono.py:33`; lote encontra carimbo falso invisível à fila admin `app/chamados/tests/test_o317_passivo_pela_porta.py:188`; dry `:256`; fora do slot `:383`; slot não derivável `:406`; outra pendência da disputa impede fechamento `:502`. |
| **9. Desligamento com histórico pendente** | Bloqueia login e fecha vínculos: `app/colaboradores/services/desligamento.py:109`, `:122`. **Resposta sem veredito preserva disputa e chamado vivo para admin julgar**; mudas podem morrer por `admin_decidiu`: `:139`, `:142`, `:155`. Cria chamado auditoria `dp_rh`: `:159`. Cartório continua julgando histórico, mas emissão exclui desligado: `app/ponto/services/cartorio.py:1696`. | Preservação resposta sem veredito e fechamento muda: `app/colaboradores/tests/test_desligar_guards.py:93`; desligado julgado sem cobrança `app/ponto/tests/test_hx_cartorio_universo.py:34`. |
| **10. DiaPago/export** | Dinheiro é atualizado pelo recalculo da competência do fato em `on_commit`: `app/ponto/services/fechamento.py:1791`, `:1800`; recalculo lavra DiaPago: `:1184`, `:1194`. Alterar só ata/veredito não equivale necessariamente a atualizar snapshot/TXT: teste exige rejulgar competência exportada deixando fechamento e TXT intactos, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:15`. Export retém por veredito/protestos da célula, com exceção preadoção e pausa exclusiva: `app/folha/export.py:1233`, `:1245`, `:1253`. **Comparações DiaPago×fechamento/espelho são publicadas, mas ficam FORA de `falhas`; não travam sozinhas o TXT**: `app/folha/porta_export.py:687`, `:696`. | `app/folha/tests/test_porta_do_export.py:135` sela contadores sem reprovação; `:216` acusa DiaPago incorreto; `:495` não lavrado distinto de zero; `app/folha/tests/test_borda_celula.py:49` sela furo retentor e `:90` preadoção sem retenção. |

Detalhes para consolidação:

- **No noturno, distinguir jornada de data civil é requisito concreto**, porque `data_evento` tanto recebe D+1 no teste do emissor de intervalo (`app/chamados/tests/test_fio_intervalo.py:77`) quanto serve de entrada para buscar os marcos e possivelmente acrescentar outro dia (`app/chamados/juizes.py:303`, `:310`). Não afirme que o fluxo atual garante “nunca 23”; a prova exata não foi encontrada [nao verificado].
- **Atestado aberto não deve ser descrito como chamado fechado**: o código explicitamente pula `reconciliar()` enquanto a ausência está em ABERTAS (`app/chamados/services/materializacao.py:562`). A absorção de cada irmã também pode ser recusada pelo C6, com trilha própria e retorno da lista de recusadas (`:549`, `:553`, `:569`).
- **Desligamento não apaga a obrigação histórica respondida**: a exceção preserva resposta sem veredito e a deixa ao admin, apesar de remover o acesso do colaborador (`app/colaboradores/services/desligamento.py:111`, `:139`, `:155`). Não substitua isso pela regra antiga “cascade fecha tudo”.
- **Export de desligado**: data de demissão até o fim da janela direciona para rescisão; demissão depois do fim não exclui aquela competência mensal (`app/folha/export.py:854`, `:858`); retenção de célula recorta universo por admissão/demissão (`app/folha/export.py:1198`).
- **Passivo validado exportado**: plantio, regeneração pela porta declarada e pauta DP são um par transacional; recusa da regeneração desfaz plantio/carimbos (`app/chamados/services/passivo_validado.py:371`, `:374`, `:419`, `:438`); pauta só quando o gravado mudou (`:442`), com tratamento próprio quando não mudou (`:445`).

## 06/11 21:28 · sessao 7e4c6131adfe · DUAS-TELAS-E-PWA-OFFLINE · censo · origem=Code

**Fechei 32 views POST próprias no recorte: 18 de ponto e 14 de férias. Elas expõem 33 rotas; colaboradores fornece duas telas que reutilizam o POST de ponto.** A contagem exclui os caminhos herdados do Django Admin, cuja implementação não está nesta cópia. `app/ponto/urls.py:37`, `app/ponto/urls.py:53`, `app/ponto/urls.py:83`, `app/ferias/urls.py:9`, `app/colaboradores/views.py:1935`, `app/colaboradores/views.py:2938`, `app/ponto/admin.py:18`; implementação herdada: **[não verificado]**.

**Censo consolidado: 10 grupos divergentes fechados e 1 par de endpoints com segundo disparador visual não confirmado.** Conto cada grupo uma vez, sem multiplicar suas combinações; os caminhos e as diferenças estão discriminados abaixo. As bases dessa contagem são as views citadas em D1–D11.

**Ocorrência, frequência e valores afetados em produção: [não verificado].** “Muda dinheiro/dia: sim” abaixo significa capacidade demonstrada pelo caminho do código, não prejuízo observado.

Todos os arquivos abreviados abaixo são relativos a `app/`; por exemplo, `ponto/views.py:1911` significa `app/ponto/views.py:1911`.

O padrão utilizado foi escritor único por estado, com célula como fonte e espelho/DiaPago/fechamento como projeções. As autoridades vivas de cobertura, efeito, saldo e fracionamento estão registradas no código. `docs/GLOSSARIO.md:11`, `docs/GLOSSARIO.md:12`, `docs/PLANO-PADROES.md:11`, `core/juizes.py:239`.

**Universo de ações e telas.**

São **25 ações pela seguinte convenção**, separando materialização retroativa, resolução do dia e registro histórico, mas agrupando operações individuais e em lote:

| App | Ações inventariadas | Views |
|---|---|---|
| Ponto | Ausência: lançar, aprovar, rejeitar, cancelar, editar registro, declarar período parcial, reverter rejeição, anexar documento. Justificativa: aprovar, rejeitar, materializar saída. Resolver dia. Criar e editar tipo. | 18 funções; referências em `ponto/views.py:1763`, `ponto/views.py:1859`, `ponto/views.py:1911`, `ponto/views.py:1923`, `ponto/views.py:2133`, `ponto/views.py:2270`, `ponto/views.py:2304`, `ponto/views.py:2333`, `ponto/views.py:2368`, `ponto/views.py:2569`, `ponto/views.py:2599`, `ponto/views.py:2662`, `ponto/views.py:2680`, `ponto/views.py:2720`, `ponto/views.py:2751`, `ponto/views.py:3106`, `ponto/views.py:3312`, `ponto/views_tipos_ausencia.py:57`. |
| Férias | Sincronizar/criar período, registrar gozo histórico, agendar, cancelar, alterar datas, aprovar solicitação, rejeitar solicitação, editar período, excluir período, enviar confirmação, vender dias. | 14 funções; `ferias/views.py:123`, `ferias/views.py:269`, `ferias/views.py:321`, `ferias/views.py:341`, `ferias/views.py:378`, `ferias/views.py:421`, `ferias/views.py:519`, `ferias/views.py:569`, `ferias/views.py:633`, `ferias/views.py:711`, `ferias/views.py:777`, `ferias/views.py:808`, `ferias/views.py:830`, `ferias/views.py:906`. |
| Colaboradores | Calendário e diagnóstico disparam “Resolver dia”; não acrescentam escritor POST local dessas entidades. | `colaboradores/views.py:1935`, `colaboradores/views.py:2938`, `templates/colaboradores/calendario_partial.html:63`, `templates/colaboradores/partials/_drawer_diagnostico.html:36`. |

As rotas dos três apps vêm de `config/urls.py:26`, `config/urls.py:29`, `config/urls.py:32`; CI repete esses includes, e o URLconf de teste herda as rotas da UI. `config/urls_ci.py:7`, `config/urls_test.py:16`.

**Leitores usados para avaliar as diferenças.**

| Leitor | O que observa |
|---|---|
| Espelho | Cobertura para cobrança e cobertura aprovada para folha; justificativas aprovadas também justificam o dia. `ponto/services/espelho.py:582`, `ponto/services/espelho.py:588`, `ponto/services/efeito_ausencia.py:40`. |
| Fechamento / DiaPago | Justificativas aprovadas, efeito da ausência e abono extraído da grade. `ponto/services/fechamento.py:660`, `ponto/services/fechamento.py:693`, `ponto/services/dia_pago.py:385`. |
| Folha | Ausências aprovadas que descontam/abonam; férias com batida são consultadas pelo **agendamento**. `folha/export.py:87`, `folha/export.py:140`, `folha/export.py:201`, `folha/export.py:491`. |
| Saldo / gozo de férias | Saldo deriva de direito−concedidos−vendidos; gozo consulta agendamento não cancelado. `ferias/models.py:216`, `ferias/services.py:47`, `ferias/services.py:57`. |

**D1 — Aprovar justificativa: individual × lote × materialização retroativa.**

| Caminho | Tela, rota, view → escritor |
|---|---|
| Individual | Painel `/ponto/justificativa/<jid>/painel/`, `ponto/views.py:1662`; `templates/ponto/partials/painel_justificativa.html:51` → POST `/ponto/justificativa/<jid>/aprovar/`, `ponto/views.py:1911` → `aprovar_justificativa_dp`, `ponto/services/justificativa.py:117`. Rotas: `ponto/urls.py:39`. |
| Lote | Fila `/ponto/justificativa/fila/`, `ponto/views.py:1695`; `templates/ponto/fila_justificativas.html:176` → POST `/ponto/justificativa/aprovar-lote/` → **escrita na própria view**, `ponto/views.py:1763`, `ponto/views.py:1796`. |
| Retroativa | `/ponto/justificativa/<jid>/materializar-saida/`, `ponto/views.py:3106`; `templates/ponto/materializar_saida_retroativa.html:38` → `materializar_saida_dp`, `ponto/services/justificativa.py:316`. Rota: `ponto/urls.py:44`. |

O individual valida saída posterior à entrada, gera saída somente quando há entrada e saída informada, fecha origem e bloqueio de entrada, chama auditoria e followup. O lote grava aprovação diretamente, pode gerar saída sem entrada e não executa aquela validação temporal nem os mesmos efeitos de bloqueio/auditoria/followup. Ambos invalidam fechamento e fecham origem. `ponto/services/justificativa.py:137`, `ponto/services/justificativa.py:190`, `ponto/services/justificativa.py:199`, `ponto/services/justificativa.py:202`, `ponto/services/justificativa.py:214`, `ponto/views.py:1796`, `ponto/views.py:1804`, `ponto/views.py:1808`, `ponto/views.py:1837`.

A retroativa aceita justificativa sem filtro de status, recebe novo horário e motivo, grava `saida_informada` e aprovação, e fecha uma cascata de chamados. Tem auditoria própria; não usa o escritor individual de aprovação. `ponto/views.py:3130`, `ponto/services/justificativa.py:329`, `ponto/services/justificativa.py:340`, `ponto/services/justificativa.py:365`, `ponto/services/justificativa.py:378`, `ponto/services/justificativa.py:398`.

**Muda dinheiro/dia: sim, potencialmente.** Os caminhos podem produzir batida e estado diferentes; aprovação é insumo do fechamento e do espelho. `ponto/services/fechamento.py:660`, `ponto/services/efeito_ausencia.py:40`.

**D2 — Rejeitar justificativa: individual × lote.**

| Tela A | Tela B |
|---|---|
| Painel, `templates/ponto/partials/painel_justificativa.html:58` → `/ponto/justificativa/<jid>/rejeitar/`, `ponto/views.py:1923` → `rejeitar_justificativa_dp`, `ponto/services/justificativa.py:247`. | Fila, `templates/ponto/fila_justificativas.html:188` → `/ponto/justificativa/rejeitar-lote/` → view/escritor `ponto/views.py:1859`. Rotas: `ponto/urls.py:42`. |

Individual usa transação/trava, cria chamado de rejeição e followup. Lote faz `get`/`save` diretamente e envia push, sem esses mesmos efeitos. O receiver comum rejulga o dia nos dois caminhos: **não há ausência de rejulgamento exclusiva do lote**. `ponto/services/justificativa.py:258`, `ponto/services/justificativa.py:275`, `ponto/services/justificativa.py:309`, `ponto/views.py:1874`, `ponto/views.py:1882`, `ponto/signals.py:161`.

**Muda dinheiro/dia: sim em concorrência, por inferência estática.** A leitura/escrita sem trava do lote pode disputar com a aprovação; o status resultante alimenta a cobertura do dia. No processamento sequencial da mesma pendência, a divergência demonstrada é chamado/followup. `ponto/views.py:1874`, `ponto/services/justificativa.py:127`, `ponto/services/efeito_ausencia.py:40`.

**D3 — Lançar ausência/falta/abono: formulário × Resolver dia.**

| Tela A | Tela B |
|---|---|
| `/ponto/ausencias/lancar/`, view `ponto/views.py:2368`; `templates/ponto/lancar_ausencia.html:12` → `criar_ausencia`, `ponto/services/ausencia.py:130`, chamada em `ponto/views.py:2415`. | Calendário `/colaboradores/<cid>/calendario/`, `colaboradores/views.py:1935`, e diagnóstico `/colaboradores/colab/<cid>/diagnostico-drawer/`, `colaboradores/views.py:2938` → formulários `templates/ponto/partials/_veredito_dia.html:17` e `_veredito_lote_barra.html:2` → `/ponto/celula/veredito/`, `ponto/views.py:3312` → `veredito_celula`, `ponto/services/veredito_celula.py:191` → `criar_ausencia`, `ponto/services/veredito_celula.py:315`. Rotas: `ponto/urls.py:57`, `colaboradores/urls.py:24`, `colaboradores/urls.py:86`. |

**Converge na criação de Ausencia**, mas o formulário permite range/minutos/documento/HE100; Resolver dia passa um dia, fecha perguntas/chamados e lavra veredito humano na célula. Para INSS, somente o lançamento chama a alteração de `Colaborador.situacao` para afastado. `ponto/views.py:2415`, `ponto/views.py:2433`, `ponto/services/veredito_celula.py:315`, `ponto/services/veredito_celula.py:325`, `ponto/services/veredito_celula.py:330`.

**Muda dinheiro/dia: sim pelo alcance/minutos; não está demonstrada diferença monetária para entradas equivalentes.** Calendário lê o carimbo humano exclusivo; DiaPago lê o abono da grade. `colaboradores/services/calendario.py:411`, `ponto/services/dia_pago.py:391`.

**D4 — Aprovar ausência: painel × lote.**

| Tela A | Tela B |
|---|---|
| `/ponto/ausencias/<aid>/painel/`, `ponto/views.py:2067`; `templates/ponto/partials/painel_ausencia.html:309` → `/ponto/ausencias/<aid>/aprovar/`, `ponto/views.py:2599`. | `/ponto/ausencias/`, `ponto/views.py:2155`; `templates/ponto/ausencias.html:117`, `templates/ponto/ausencias.html:308` → `/ponto/ausencias/lote/aprovar/`, `ponto/views.py:2270`. Rotas: `ponto/urls.py:54`, `ponto/urls.py:65`. |

Ambos convergem em `_a4_decidir` → `decidir` → `mudar_estado`. Individual expande raiz/irmãos, admite período parcial e confirmação de desconto, usa transação para os irmãos e envia push. Lote processa somente IDs selecionados, sem período e com `confirmado=False`. `ponto/views.py:82`, `ponto/services/ausencia.py:897`, `ponto/services/ausencia.py:808`, `ponto/views.py:2612`, `ponto/views.py:2615`, `ponto/views.py:2629`, `ponto/views.py:2641`, `ponto/views.py:2646`, `ponto/views.py:2282`.

**Muda dinheiro/dia: sim.** Podem ser aprovados conjuntos diferentes de registros; somente o individual declara o intervalo parcial nesse ato. Cobertura de folha aceita apenas aprovadas, e há autoridade específica para o período parcial. `ponto/turnos.py:2514`, `ponto/turnos.py:2614`.

**D5 — Rejeitar ausência: painel × endpoint de lote — segunda tela incompleta.**

Painel: `templates/ponto/partials/painel_ausencia.html:330` → `/ponto/ausencias/<aid>/rejeitar/`, `ponto/views.py:2680`. Lote: `/ponto/ausencias/lote/rejeitar/`, `ponto/views.py:2304`; existe emissor JS em `templates/ponto/ausencias.html:316`, mas botão que o invoque: **[não verificado]**. Rotas: `ponto/urls.py:55`, `ponto/urls.py:74`.

Ambos convergem em `decidir`; individual expande irmãos e envia push, enquanto lote usa IDs e persiste motivo em `descricao`. `ponto/views.py:2688`, `ponto/views.py:2701`, `ponto/views.py:2704`, `ponto/views.py:2311`, `ponto/views.py:2319`.

**Muda dinheiro/dia: sim pelo conjunto rejeitado**, pois rejeitada deixa de cobrir folha/cobrança; o par de telas permanece incompleto. `ponto/turnos.py:2514`, `ponto/turnos.py:2516`.

**D6 — Conceder férias: agendamento × solicitação × histórico.**

| Caminho | Tela, rota, view → escritor |
|---|---|
| Agendamento | `templates/ferias/agendar.html:13` ou `_secao_colab.html:60` → `/ferias/periodo/<id>/agendar/`, `ferias/views.py:269`; lote `templates/ferias/agendar_lote.html:24` → `/ferias/agendar-lote/`, `ferias/views.py:569`. Ambos → `executar_agendamento`, `ferias/services.py:306`. |
| Solicitação | `templates/ferias/solicitacoes.html:26` → `/ferias/solicitacoes/<sid>/avaliar/`, `ferias/views.py:711` → `aprovar_solicitacao`, `ferias/services.py:410`. |
| Histórico | Drawer `templates/ferias/_secao_colab.html:22` → `/ferias/<cid>/novo-periodo/`, `ferias/views.py:123`, ramo `ferias/views.py:197` → `registrar_gozo_historico`, `ferias/services.py:270`. |

Rotas acima: `ferias/urls.py:10`, `ferias/urls.py:12`, `ferias/urls.py:15`, `ferias/urls.py:16`.

Agendamento padrão usa validação comum de saldo/fracionamento/duplicata e unidade atômica. Solicitação grava aprovação antes de verificar disponibilidade e pode terminar aprovada **sem agendamento**, com aviso. Histórico possui escritor próprio, não chama o `clean` de fracionamento e limita concedidos por `min(direito, concedidos+dias)`. `ferias/views.py:226`, `ferias/services.py:315`, `ferias/services.py:416`, `ferias/services.py:424`, `ferias/services.py:428`, `ferias/services.py:443`, `ferias/services.py:286`, `ferias/services.py:299`.

Os três salvam `AgendamentoFerias`, passando pelo veto do modelo e pelo espelhamento de ausências: o histórico **não está sem veto de colisão**. `ferias/models.py:387`, `ferias/models.py:434`.

**Muda dinheiro/dia: sim, potencialmente**, pela existência do agendamento, saldo consumido e cobertura materializada; folha consulta agendamento para férias com batida. `ferias/models.py:216`, `ponto/services/ausencia.py:1202`, `folha/export.py:491`.

**D7 — Conceder férias: ausência direta × caminhos de agendamento de D6.**

A tela de lançamento de D3 aceita o tipo recebido; Resolver dia também o encaminha. Ambos criam **somente Ausencia**. Agendamento padrão, solicitação materializada e histórico criam Agendamento e alteram concedidos. `ponto/views.py:2415`, `ponto/services/veredito_celula.py:315`, `ponto/services/ausencia.py:341`, `ferias/services.py:321`, `ferias/services.py:434`, `ferias/services.py:298`.

O catálogo inclui `ferias`, e os choices não o excluem; o calendário exclui somente `falta`. A oferta efetiva desse tipo no cadastro de produção é **[não verificado]**. `ponto/catalogo/ausencias.py:101`, `ponto/catalogo/ausencias.py:303`, `ponto/catalogo/ausencias.py:313`, `colaboradores/views.py:1959`.

**Muda dinheiro/dia: sim, se férias estiver disponível.** Pode existir cobertura abonada sem consumo do saldo nem participação no universo de agendamentos que a folha usa para retenção. Não há sincronização inversa nesses caminhos; os receivers de ausência cuidam de célula/perguntas/chamados. `ferias/models.py:216`, `ponto/signals.py:143`, `ponto/signals.py:250`, `folha/export.py:491`, `folha/export.py:868`.

**D8 — Cancelar férias: normal × admin × massa × concluída.**

| Caminho | Tela, rota, view → escritor |
|---|---|
| Normal | Saldo `templates/ferias/saldo.html:71` ou drawer `_secao_colab.html:160` → `/ferias/agendamento/<id>/cancelar/`, `ferias/views.py:321` → `cancelar_agendamento_dp`, `ferias/services.py:330`. |
| Admin | Drawer `_secao_colab.html:162` → `/ferias/agendamento/<id>/cancelar-admin/`, `ferias/views.py:341` → `executar_cancelamento_admin`, `ferias/services.py:341`. |
| Massa | `templates/ferias/cancelar_admin_massa.html:27` → `/ferias/cancelar-admin-massa/`, `ferias/views.py:519` → mesmo escritor admin, chamada `ferias/views.py:559`. |
| Concluída | Drawer `_secao_colab.html:166` → `/ferias/agendamento/<id>/corrigir-concluida/`, `ferias/views.py:378` → mesmo escritor admin, chamada `ferias/views.py:407`. |

Rotas: `ferias/urls.py:9`, `ferias/urls.py:18`, `ferias/urls.py:19`, `ferias/urls.py:20`.

Normal bloqueia somente `concluido`; portanto uma repetição com status cancelado chega novamente ao decremento de concedidos. Admin recusa cancelado; concluída exige estado próprio e justificativa. Normal não envolve suas duas gravações no `atomic` que existe no escritor admin. `ferias/views.py:325`, `ferias/services.py:333`, `ferias/models.py:335`, `ferias/views.py:396`, `ferias/views.py:399`, `ferias/services.py:352`.

Admin carimba `obs` e rejeita ausências antes de removê-las; individual admin e concluída chamam LogAuditoria, massa não faz a mesma chamada. **Todos removem as ausências no resultado final do cancelamento**, pelo `save` do agendamento. `ferias/services.py:356`, `ferias/services.py:358`, `ferias/views.py:367`, `ferias/views.py:409`, `ferias/views.py:551`, `ferias/models.py:434`, `ponto/services/ausencia.py:1188`.

**Muda dinheiro/dia: sim, sobretudo saldo/idempotência.** O segundo POST normal volta a tentar devolver dias; o saldo depende diretamente desse contador. A disponibilidade visual também diverge: saldo exige apenas status agendado, drawer exige `pode_cancelar`, incluindo ainda não ter iniciado. `ferias/services.py:333`, `ferias/models.py:216`, `templates/ferias/saldo.html:70`, `templates/ferias/_secao_colab.html:159`, `ferias/models.py:330`.

**D9 — Alterar datas de férias: edição normal × correção admin.**

| Tela A | Tela B |
|---|---|
| Drawer `_secao_colab.html:130` → `/ferias/agendamento/<id>/editar/`, `ferias/views.py:633` → `remarcar_agendamento`, `ferias/services.py:403`. | Drawer `_secao_colab.html:146` → `/ferias/agendamento/<id>/editar-datas-admin/`, `ferias/views.py:421` → `corrigir_datas_agendamento`, `ferias/services.py:398`. Rotas: `ferias/urls.py:21`, `ferias/urls.py:22`. |

**Converge em `_trocar_janela`**, que altera datas/status/aviso, limpa ausências antigas e atualiza concedidos atomicamente. Normal exige status agendado; admin admite qualquer não cancelado, exige motivo e registra LogAuditoria. `ferias/services.py:366`, `ferias/services.py:377`, `ferias/views.py:649`, `ferias/views.py:452`, `ferias/views.py:454`, `ferias/views.py:497`.

**Muda dinheiro/dia: sim pelo universo permitido**, pois admin pode mover férias iniciadas/concluídas. Para a mesma alteração aceita, o escritor das datas está unificado. `ferias/services.py:398`, `ferias/services.py:403`, `ferias/views.py:452`.

**D10 — Alterar férias: editar Ausencia × editar Agendamento.**

Ausência: painel `/ponto/ausencias/<aid>/painel/`, `ponto/views.py:2067`, formulário `templates/ponto/partials/painel_ausencia.html:134` → `/ponto/ausencias/<aid>/editar/`, `ponto/views.py:2751` → `editar_ausencia_registro`, `ponto/services/ausencia.py:465`. Compare com os dois caminhos de D9. Rota: `ponto/urls.py:76`.

Editar Ausencia altera tipo/range/minutos e invalida fechamento; não altera Agendamento/concedidos. Editar Agendamento usa `_trocar_janela`, alterando os dois lados e limpando a janela antiga. `ponto/services/ausencia.py:498`, `ponto/services/ausencia.py:509`, `ponto/services/ausencia.py:524`, `ferias/services.py:378`, `ferias/services.py:387`.

**Muda dinheiro/dia: sim.** A cobertura do espelho pode mudar enquanto saldo e intervalo consultado pela folha permanecem no agendamento antigo. `ponto/services/espelho.py:588`, `ferias/models.py:216`, `folha/export.py:491`.

**D11 — Desfazer férias: cancelar Ausencia × cancelar Agendamento.**

Lista de ausências `templates/ponto/ausencias.html:120`, `templates/ponto/ausencias.html:278` → `/ponto/ausencias/lote/cancelar/`, `ponto/views.py:2333` → `decidir`, `ponto/services/ausencia.py:897`. Compare com os quatro caminhos de D8. Rota: `ponto/urls.py:56`.

O lote aceita qualquer ausência não rejeitada, inclusive férias, e a rejeita; não devolve concedidos nem cancela Agendamento. O cancelamento de Agendamento altera o saldo/status e remove os espelhos. `ponto/views.py:2345`, `ponto/views.py:2351`, `ferias/services.py:333`, `ferias/services.py:353`, `ponto/services/ausencia.py:1188`.

**Muda dinheiro/dia: sim.** Cancelar somente Ausencia retira cobertura, mas o Agendamento continua no universo de gozo/retenção e o saldo não recebe devolução. `ponto/turnos.py:2514`, `ferias/services.py:51`, `ferias/models.py:216`, `folha/export.py:868`.

**Pares que convergem no mesmo escritor — separados para fechar o censo.**

| Par/grupo | Convergência e diferenças restantes |
|---|---|
| Resolver dia no calendário × diagnóstico; individual × seleção em lote | **Converge em `veredito_celula`**, pelo mesmo POST. Mesmos efeitos de ausência/fio/célula

[truncado em 20000 de 24964 caracteres]

## 06/11 21:27 · sessao 6868c22bd168 · - · - · origem=Ronald (tmux)

**C — identidade, banco e concorrência**

- **Pergunta:** chave de dedup real = `(disputa.colaborador_id, motivo, data_evento)`, atravessando todas as disputas e estados; lê em ordem de PK e reutiliza a última linha. Não inclui posição/hora do marco nem instância da tarefa. Perguntas chamadas sem data são tratadas como conversa e nascem sempre. `app/chamados/models.py:2381`, `app/chamados/models.py:2392`, `app/chamados/models.py:2398`, `app/chamados/models.py:2424`, `app/chamados/models.py:2431`.
- **Dois marcos usuais podem distinguir-se hoje:** fim/S vira `orfao_14h`; intervalo/S vira `intervalo_saida`; início/E e retorno/E também têm motivos diferentes. A distinção é indireta pelo motivo, não uma chave explícita de MARCO. Dois marcos com o mesmo motivo e data caem no mesmo dedup. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:2392`.
- **Data não é uniformemente jornada:** fábrica de intervalo toma a data civil fornecida no marco; teste noturno espera as duas perguntas no dia seguinte ao `data_turno`. `app/chamados/services/disputa_emissao.py:982`, `app/chamados/services/disputa_emissao.py:987`, `app/chamados/services/disputa_emissao.py:991`, `app/chamados/tests/test_fio_intervalo.py:62`, `app/chamados/tests/test_fio_intervalo.py:77`.
- **Disputa:** busca a aberta por colaborador, agrega motivos e contexto; não constitui tarefa individual por marco. Meta contém índice comum `(colaborador, fechada_em)`, sem unicidade parcial. `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:472`, `app/chamados/services/disputa_emissao.py:498`, `app/chamados/models.py:1742`.
- **Chamado:** não possui chave única universal; `abrir` usa os `**lookup` recebidos de cada emissor. `unique_together=[]`; os índices declarados são comuns, de estado/urgência/criação. Exemplo do pai de disputa: colaborador + categoria + módulo + estados vivos. `app/chamados/models.py:97`, `app/chamados/models.py:796`, `app/chamados/models.py:867`, `app/chamados/services/disputa_emissao.py:504`.
- **Não achei índice único parcial de tarefa aberta** em `ChamadoColaborador.Meta`, `DisputaSupervisao.Meta`, `PerguntaDisputa.Meta` nem no censo de constraints das migrations de chamados. Existe unique parcial em **Followup**, sobre `evento_chave` não nulo; não é identidade da tarefa. `app/chamados/models.py:97`, `app/chamados/models.py:1742`, `app/chamados/models.py:2857`, `app/chamados/migrations/0047_evento_chave_do_fio.py:20`.
- **Há locks hoje:** `ChamadoColaborador.abrir` abre atomic e trava a linha do colaborador antes do dedup; encerramento recarrega/trava a linha do chamado dentro de atomic; costura trava o destino se o chamador está em atomic. A emissão trava disputa aberta existente. Portanto seria incorreto dizer “não há select_for_update”. `app/chamados/models.py:863`, `app/chamados/models.py:866`, `app/chamados/models.py:668`, `app/chamados/services/fio.py:758`, `app/chamados/services/disputa_emissao.py:466`.
- **Corridas possíveis, por inspeção:** pergunta faz `.last()` e depois `create()` sem atomic/lock no próprio gate; disputa inexistente não oferece linha ao `select_for_update`, e o caminho cria depois de chamar `Chamado.abrir`, sem repetir a busca da disputa após adquirir o lock do colaborador; validação lê e testa `validada_em` antes do atomic, sem lock da pergunta; fechamento da disputa verifica estado/perguntas antes do atomic e depois salva sem lock. São riscos estruturais, não ocorrências demonstradas pelo anexo. `app/chamados/models.py:2431`, `app/chamados/models.py:2476`, `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:523`, `app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:366`, `app/chamados/services/validacao.py:395`, `app/chamados/models.py:1820`, `app/chamados/models.py:1827`, `app/chamados/models.py:1846`.
- **Teste de concorrência encontrado:** duas threads no `Chamado.abrir`, espera exatamente um chamado e apenas uma criação. Não achei teste equivalente para duas validações/fechamentos simultâneos na busca por concorrência/race e locks desses serviços. `app/chamados/tests/test_c2_corrida_do_abrir.py:94`, `app/chamados/tests/test_c2_corrida_do_abrir.py:127`, `app/chamados/tests/test_c2_corrida_do_abrir.py:140`; equivalência para pergunta/disputa `[nao verificado]`.

**D — fechamento e recusa**

- **Hoje validação sozinha já mata a pergunta para o C6:** primeiro ramo de `_pergunta_viva_calc` retorna falso se `validada_em` existe; `estado` mostra `validada` sem materialização e `resolvida` com ambos os carimbos. `app/chamados/juizes.py:1135`, `app/chamados/models.py:2499`.
- **O caminho normal de hora tem salvaguarda concreta adicional:** materializa em atomic, exige ramo bem-sucedido e `_pergunta_materializada_de_fato`; reprovação lança exceção para rollback; depois aplica carimbo e pergunta ao C6, também revertendo a batida se recusado. `app/chamados/services/materializacao.py:1195`, `app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1214`, `app/chamados/services/materializacao.py:1252`.
- **Essa consulta ao C6 não prova, isoladamente, pós-condição independente da validação:** `carimbar(resposta='validar')` pode preencher validação antes de perguntar ao juiz, cujo primeiro ramo já retorna morta pela validação. A checagem de fato anterior no materializador é que acrescenta a testemunha nesse caminho. `app/chamados/models.py:2591`, `app/chamados/models.py:2596`, `app/chamados/juizes.py:1135`, `app/chamados/services/materializacao.py:1208`.
- **Recusa de hora sem carimbo:** `validar_pergunta` carimba e executa a unidade dentro do mesmo atomic; `devolver_validacao_recusada` limpa `validada_em/por`, preserva a resposta, registra motivo e reabre disputa fechada por padrão. Resultado normal = `respondida`, viva para o **admin**, não nova cobrança de resposta ao colaborador. `app/chamados/services/validacao.py:180`, `app/chamados/services/validacao.py:187`, `app/chamados/services/validacao.py:199`, `app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:422`, `app/chamados/services/validacao.py:460`; prova em `app/chamados/tests/test_o261_recusa_fala.py:95`.
- **Não generalizar essa devolução:** só vale para resposta de hora, com `materializada_em` nulo e recusa preenchida. Colisão/paridade já carimbadas e escapes não entram; `sem_reabrir=True` também permite devolver ao admin sem reabrir a disputa. `app/chamados/services/validacao.py:182`, `app/chamados/services/validacao.py:184`, `app/chamados/services/validacao.py:197`, `app/chamados/services/validacao.py:427`, `app/chamados/tests/test_o261_recusa_fala.py:190`, `app/chamados/tests/test_o261_recusa_fala.py:203`.
- **Alcance não é garantido apenas por “viva”:** resposta não validada dá dono `admin_validar`; crédito vai ao destino vivo ou ao pai, mas lista padrão exclui arquivado/histórico. Testes cobrem pergunta respondida sob destino fechado e pergunta fora do app, portanto estado e porta alcançável precisam ser conferidos separadamente. `app/chamados/catalogo/natureza.py:48`, `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:60`, `app/chamados/services/verbos.py:18`, `app/chamados/services/verbos.py:48`, `app/chamados/tests/test_o261e_f5_alcance.py:169`, `app/chamados/tests/test_o261e_f5_alcance.py:205`.
- **Disputa fecha normalmente** quando não existe resposta sem veredito nem fio vivo; materializador lê esse juiz e chama `fechar`. Há exceções explícitas: desligamento, admin com motivo, competência trancada; não são todas “efeito materializado + rejulgamento”. `app/chamados/models.py:1783`, `app/chamados/models.py:1789`, `app/chamados/models.py:1722`, `app/chamados/models.py:1852`, `app/chamados/services/materializacao.py:1288`.
- **Chamado não fecha automaticamente só pelo carimbo:** destino/clones passam por retenção de irmãs respondidas sem consumo e `reconciliar()`, que consulta premissa do furo. `app/chamados/services/materializacao.py:736`, `app/chamados/services/materializacao.py:746`, `app/chamados/models.py:680`.

**Compatibilidade do desenho com absorção de ausência**

- Pode separar fechamento de tarefa e avaliação, mas precisa preservar o identificador do ato de absorção ou substituí-lo explicitamente: a recuperação atual agrupa `(disputa_id, data_evento, materializada_em)`, procura um escape do tipo da ausência no grupo e devolve as irmãs sem batida viva originada nelas. `app/chamados/services/materializacao.py:585`, `app/chamados/services/materializacao.py:605`, `app/chamados/services/materializacao.py:609`, `app/chamados/services/materializacao.py:616`.
- **Quebraria:** deixar `materializada_em` nulo até aprovação/rejulgamento final de um atestado aberto, alterar cada irmã para timestamp diferente, mover parte do grupo para outra disputa ou apagar o escape do grupo: o filtro/grupo deixa de encontrar as absorvidas. Atualmente todas recebem o mesmo `agora`, inclusive com ausência ainda aberta; o chamado segue vivo/silenciado nessa fase. `app/chamados/services/materializacao.py:529`, `app/chamados/services/materializacao.py:549`, `app/chamados/services/materializacao.py:560`, `app/chamados/services/materializacao.py:607`, `app/chamados/services/materializacao.py:609`, `app/chamados/services/materializacao.py:614`.
- **Rejeição:** sem outra ausência cobrindo, desfaz absorção e reabre as disputas para devolver perguntas ao colaborador. Teste cobre pendência, vivacidade, disputa aberta, idempotência, outra ausência, aprovação sem devolução, batida já plantada e reabertura da mesma ausência. `app/chamados/services/materializacao.py:638`, `app/chamados/services/materializacao.py:645`, `app/chamados/services/materializacao.py:657`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:106`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:119`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:129`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:141`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:154`.

Testes foram somente lidos; resultados de execução `[nao verificado]`. [nao verificado]

## 06/11 21:27 · sessao d20cb5a00918 · - · - · origem=Ronald (tmux)

E6 — concorrência:

- O chamado já tem proteção parcial: `abrir` trava `Colaborador` antes do dedup; fechamento recarrega e trava o chamado dentro de transação; costura trava a mesma linha do destino. Isso serializa abertura e disputa entre costura e encerramento pelos caminhos protegidos (`app/chamados/models.py:863`, `app/chamados/models.py:661`, `app/chamados/services/fio.py:752`).
- A disputa ainda tem janela: `fechar` testa estado, respostas e perguntas antes de entrar no `atomic`; grava sem `select_for_update` ou recarga. Duas decisões sobre instâncias antigas podem divergir — inferência do fluxo (`app/chamados/models.py:1820`, `app/chamados/models.py:1827`, `app/chamados/models.py:1836`, `app/chamados/models.py:1846`, `app/chamados/models.py:1861`).
- A emissão trava somente uma disputa aberta já existente; na ausência dela, abre/reusa o pai e cria disputa sem repetir a consulta. Duas consultas inicialmente vazias podem criar duas disputas após o reuso do mesmo pai — inferência, sem reprodução (`app/chamados/services/disputa_emissao.py:466`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:523`). O modelo declara índice comum `(colaborador, fechada_em)`, sem unicidade nesse `Meta` (`app/chamados/models.py:1742`).
- Há teste real com duas threads para **abertura de chamado**, exigindo um único chamado e resultados `[False, True]`; não equivale a testar duas validações/fechamentos simultâneos (`app/chamados/tests/test_c2_corrida_do_abrir.py:94`, `app/chamados/tests/test_c2_corrida_do_abrir.py:127`, `app/chamados/tests/test_c2_corrida_do_abrir.py:140`).
- Há teste de SQL/ordem de lock para gates e costura, e teste com instância velha, mas sem duas threads nesses testes (`app/chamados/tests/test_o261e_f1_codex_v5.py:24`, `app/chamados/tests/test_o261e_f1_codex_v5.py:49`, `app/chamados/tests/test_o261e_f1_codex_v5.py:65`).
- Não achei teste real de duas transições simultâneas de pergunta/disputa em `test_porta_disputa_emissao.py`, `test_o261e_f1_codex_v5.py` e `test_pvmuda_carimbo_em_voo.py`; este último exercita sequências por máquina de estados, não threads (`app/chamados/tests/test_porta_disputa_emissao.py:185`, `app/chamados/tests/test_o261e_f1_codex_v5.py:24`, `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:178`).

E7 — falha entre evento e entrega:

- Já existe outbox durável `EventoCelula`; registra identificadores/data e releitura do estado atual. Os índices parciais são **não únicos**, para pendentes; não é índice único de tarefa aberta (`app/escala/models.py:1845`, `app/escala/models.py:1857`, `app/escala/models.py:1865`).
- Célula salva veredito/ata e insere evento no mesmo `atomic`; falha do INSERT desfaz a escrita (`app/ponto/portas/celula.py:894`). `PerguntaDisputa.save` também envolve o `post_save` emissor em `atomic`, com savepoint; nascimento não emite, alterações relevantes emitem (`app/chamados/models.py:2847`, `app/chamados/signals.py:214`).
- Cobertura explícita: rollback célula/evento; rollback pergunta/evento; reconsumo idempotente e falha parcial que deixa pendência para a próxima execução (`app/ponto/tests/test_o_outbox_celula.py:142`, `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:300`, `app/ponto/tests/test_o_outbox_celula.py:163`, `app/ponto/tests/test_o_outbox_celula.py:209`).
- Consumidor processa o estado atual; falha incrementa tentativa/erro e mantém pendente, sucesso marca `processado_em`; após cinco tentativas o evento sai do lote vivo e passa a “morto”. Adiamento exclusivamente pelo teto mantém pendente sem consumir tentativa (`app/chamados/reconciliador.py:1196`, `app/chamados/reconciliador.py:1235`, `app/chamados/reconciliador.py:1304`, `app/chamados/reconciliador.py:1377`).
- O comando usa advisory lock por schema; chamar diretamente `consumir_eventos_celula` não adquire esse lock no corpo da função (`app/chamados/management/commands/consumir_eventos_celula.py:17`, `app/chamados/reconciliador.py:1213`, `app/chamados/reconciliador.py:1304`).
- Nem todo efeito secundário está no outbox: devolução de perguntas absorvidas por ausência e reconciliação são `on_commit` com blocos `engolir`; falha não desfaz ausência já gravada nem há reentrega durável nesses blocos — inferência local (`app/ponto/signals.py:250`, `app/ponto/signals.py:260`, `app/ponto/signals.py:272`). Recalcular fechamento na validação captura exceção e segue (`app/chamados/services/validacao.py:405`).
- Cuidado com o teste chamado `test_a_trava_real_exclui_a_segunda_sessao...`: ele entra duas vezes **na mesma sessão**, espera sucesso reentrante, e deixa a exclusão entre sessões como comentário; não demonstra duas sessões (`app/ponto/tests/test_o_outbox_celula.py:497`).

Sobre a ordem proposta:

- Já há vigia lendo pergunta × chamado × célula e donos de ação; ele não é um process manager de tarefa novo (`app/chamados/services/vigia_fio.py:4`, `app/chamados/services/vigia_fio.py:16`, `app/chamados/services/vigia_fio.py:169`).
- O universo do vigia preserva passivo por partir dos chamados vivos, sem janela temporal; isso é comportamento declarado do módulo, não prova dos números históricos citados em comentários (`app/chamados/services/vigia_fio.py:35`).
- O fluxo atual já põe julgamento antes do consumidor da batida e outbox antes dos antigos crons de cura; o cron `reconciliar_chamados` tem teste impedindo voltar a drenar o outbox (`app/ponto/signals.py:169`, `app/chamados/reconciliador.py:1347`, `app/ponto/tests/test_o_outbox_celula.py:482`).
- Não achei implementação ou teste da sequência integral “nova tarefa → sombra → app/admin no mesmo pouso → retirada dos ponteiros” nas áreas inspecionadas; tratar aprovação dessa ordem como recomendação do desenho, `[nao verificado]`. [nao verificado]

Testes existentes foram lidos, não executados; resultados nesta cópia `[nao verificado]`. [nao verificado]

## 06/11 21:27 · sessao 10c9fd12abfa · DUAS-TELAS-E-PWA-OFFLINE · censo · origem=Code

**Resultado: 33 views com escrita POST administrativa no recorte; 36 ações semânticas; 10 grupos divergentes com telas identificadas.** Há ainda dois cruzamentos entre escritores de Batida e nove ações que convergem, relacionados abaixo. A contagem distingue ação de endpoint: aprovação individual/lote conta uma ação; assumir/liberar contam duas; criar regularização em colaboradores repete a ação existente em chamados. Fontes do inventário: `app/chamados/urls.py:8`, `app/ponto/urls.py:8`, `app/colaboradores/urls.py:87`, `app/api/urls.py:9`.

**Ocorrências, pessoas afetadas, valores e execução em produção: [não verificado].** Os impactos abaixo são possibilidades demonstradas pelos caminhos do código. A referência adotada é escritor único, conforme `app/docs/GLOSSARIO.md:12`, `app/docs/PLANO-PADROES.md:46` e `app/docs/JUIZES.md:37`.

**Universo varrido**

| App | Views POST administrativas do recorte |
|---|---|
| chamados — **22** | `views.py`: abrir:147, followup:274, detalhe local:359, fio:659, modal:869, mensagem rápida:961, atribuir:1008, conferir data:1292, reabrir questionário:1372. `views_cobrar.py`: cobrar:24, massa:61, validar inline:145, validar lote:232, cobrar dia:306, validar lote tela:372. `views_arquivar.py`: arquivar:24, desarquivar:52, arquivar lote:93. `views_regularizacao.py`: criar:42, adicionar item:74, excluir item:137, enviar:154. Todos sob `app/chamados/`. |
| ponto — **10** | Offline pendentes:434; aprovar lote:1763; rejeitar lote:1859; aprovar individual:1911; rejeitar individual:1923; materializar saída:3106; flip:3260; retratar:3274; desfazer:3286; resolver dia:3312. Todas em `app/ponto/views.py`. |
| colaboradores — **1** | `colab_regularizar`, `app/colaboradores/views.py:2948`. |
| api — **0** | As escritas pertinentes operam sobre o próprio colaborador: `app/api/views.py:2608`, `app/api/views.py:2853`, `app/api/views.py:2998`, `app/api/views_core.py:1171`, `app/api/views_core.py:1316`. Os endpoints mensageria “validar”, “decidir” e “resolver-dia-recusado” são leitores: `app/api/views_mensageria.py:523`, `app/api/views_mensageria.py:1134`, `app/api/views_mensageria.py:1847`. |

**Endereços das telas usados nos pares**

| Sigla | Rota → view → template |
|---|---|
| **D** | `/chamados/local/<id>/` → `detalhe_chamado_local`, `app/chamados/views.py:359` → `chamados/detalhe_local.html`, render em `app/chamados/views.py:550`. |
| **M** | `/chamados/modal/<colab>/` → `modal_fio_colaborador`, `app/chamados/views.py:869` → `chamados/partials/modal_fio.html`, render em `app/chamados/views.py:949`; rota em `app/chamados/urls.py:39`. |
| **F** | `/chamados/colaborador/<colab>/` → `fio_colaborador`, `app/chamados/views.py:659` → `chamados/fio_colaborador.html`, render em `app/chamados/views.py:697`. |
| **T** | `/chamados/local/<id>/painel/` → `painel_chamado_turno`, `app/chamados/views.py:347` → `chamados/partials/painel_chamado_turno.html`, render em `app/chamados/views.py:353`. |
| **C** | `/colaboradores/<cid>/calendario/` → `calendario_ponto`, `app/colaboradores/views.py:1935` → `colaboradores/calendario_partial.html`, render em `app/colaboradores/views.py:1962`. |
| **G** | `/colaboradores/colab/<cid>/diagnostico-drawer/` → `colab_diagnostico_drawer`, `app/colaboradores/views.py:2938` → `colaboradores/partials/_drawer_diagnostico.html`, render em `app/colaboradores/views.py:2942`. |
| **J** | `/ponto/justificativa/<jid>/painel/` → `painel_justificativa`, `app/ponto/views.py:1662` → `ponto/partials/painel_justificativa.html`; formulários em `app/templates/ponto/partials/painel_justificativa.html:51`. |
| **L** | `/ponto/justificativa/fila/` → `fila_justificativas`, `app/ponto/views.py:1695` → `ponto/fila_justificativas.html`, render em `app/ponto/views.py:1749`. |

**Grupos divergentes**

1. **Aprovar justificativa / materializar sua saída — três caminhos.**

   **A: J**, formulário `app/templates/ponto/partials/painel_justificativa.html:51` → POST `/ponto/justificativa/<jid>/aprovar/`, view `app/ponto/views.py:1911` → escritor `aprovar_justificativa_dp`, `app/ponto/services/justificativa.py:117`.

   **B: L**, formulário de lote `app/templates/ponto/fila_justificativas.html:176` → POST `/ponto/justificativa/aprovar-lote/`, view/escritor `app/ponto/views.py:1763`; grava diretamente em `app/ponto/views.py:1796`.

   **C:** `/ponto/justificativa/<jid>/materializar-saida/`, view `app/ponto/views.py:3106`, template `app/templates/ponto/materializar_saida_retroativa.html:38` → escritor `materializar_saida_dp`, `app/ponto/services/justificativa.py:316`.

   **Difere:** A exige saída posterior à entrada e só cria S quando existe entrada; B não faz essa comparação e cria S tendo saída informada, mesmo sem entrada. A retorna antes de aprovar quando já existe batida gerada; B aprova e pula apenas a criação. Fontes: `app/ponto/services/justificativa.py:137`, `app/ponto/services/justificativa.py:169`, `app/ponto/views.py:1796`, `app/ponto/views.py:1808`.

   A fecha também bloqueio de entrada e acrescenta LogAuditoria/followup; B encerra a origem sem esses atos explícitos. C exige motivo de dez caracteres, não exige status pendente, grava a saída informada e executa cascata dos chamados do turno. Fontes: `app/ponto/services/justificativa.py:203`, `app/ponto/services/justificativa.py:214`, `app/ponto/views.py:1837`, `app/ponto/views.py:3135`, `app/ponto/services/justificativa.py:329`, `app/ponto/services/justificativa.py:365`, `app/ponto/services/justificativa.py:378`.

   **Leitor/gravidade: sim, pode mudar dia e dinheiro.** O fechamento inclui justificativas aprovadas em `datas_justificadas`; o motor usa isso para falta, abono e DSR. Fontes: `app/ponto/services/fechamento.py:660`, `app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:2620`.

2. **Rejeitar justificativa — individual × lote.**

   **A: J**, formulário `app/templates/ponto/partials/painel_justificativa.html:58` → POST `/ponto/justificativa/<jid>/rejeitar/`, view `app/ponto/views.py:1923` → escritor `app/ponto/services/justificativa.py:247`.

   **B: L**, formulário `app/templates/ponto/fila_justificativas.html:188` → POST `/ponto/justificativa/rejeitar-lote/`, view/escritor `app/ponto/views.py:1859`.

   **Difere:** A usa transação, trava a origem e a justificativa, abre chamado de rejeição e acrescenta followup. B faz leitura/save diretamente e envia push, sem esses mesmos atos. Fontes: `app/ponto/services/justificativa.py:258`, `app/ponto/services/justificativa.py:262`, `app/ponto/services/justificativa.py:276`, `app/ponto/services/justificativa.py:309`, `app/ponto/views.py:1874`, `app/ponto/views.py:1880`.

   **Leitor/gravidade:** o fio recebe chamado/followup somente pelo caminho individual; todo save também rejulga o dia. Na execução isolada sobre o mesmo pendente, ambos gravam rejeitada; **mudança adicional de dinheiro por concorrência é [não verificado]**, embora o lote não tenha a mesma proteção contra decisão concorrente. Fontes: `app/ponto/services/justificativa.py:268`, `app/ponto/views.py:1875`, `app/ponto/signals.py:161`.

3. **Validar pergunta — detalhe × modal × inline × lote.**

   **A: D**, formulário `app/templates/chamados/partials/_pergunta_card.html:92` → ramo `app/chamados/views.py:483`.

   **B: M**, formulário `app/templates/chamados/partials/modal_fio.html:274` → `processar_post_fio`, `app/chamados/services/fio.py:581`.

   **C: Central `/chamados/painel/`**, view `app/chamados/views.py:721`, botão `app/templates/chamados/partials/_lista_chamados.html:82` → `/chamados/validar-inline/<cid>/`, view `app/chamados/views_cobrar.py:145`.

   **D: prévia de lote**, template `app/templates/chamados/partials/_lote_preview.html:21` → `/chamados/validar-lote/tela/`, view `app/chamados/views_cobrar.py:372`; copiloto também chama `/chamados/validar-lote/`, view `app/chamados/views_cobrar.py:232`, por `app/templates/core/_copiloto.html:208`.

   **Converge em `validar_pergunta`, `app/chamados/services/validacao.py:343`, mas difere antes do escritor:** detalhe exige disputa aberta; modal aceita pergunta viva em disputa fechada, passa `sem_reabrir=True` e acrescenta trilha específica. Inline percorre as respondidas vivas da disputa; lote só aceita classe A e reclassifica cada item. Fontes: `app/chamados/views.py:493`, `app/chamados/services/fio.py:596`, `app/chamados/services/fio.py:605`, `app/chamados/services/fio.py:611`, `app/chamados/views_cobrar.py:169`, `app/chamados/services/validacao.py:688`.

   **Leitor/gravidade: sim, pode mudar dia e dinheiro**, porque a diferença de alcance decide se a resposta chega à materialização e ao recálculo. Fontes: `app/chamados/services/validacao.py:406`, `app/chamados/services/validacao.py:423`, `app/ponto/services/espelho.py:1022`, `app/ponto/services/fechamento.py:1193`.

4. **Regularizar/fechar disputa — detalhe × modal.**

   **A: D**, formulário `app/templates/chamados/detalhe_local.html:315` → `app/chamados/views.py:467` → orquestrador `app/chamados/services/acoes_disputa.py:118`.

   **B: M**, formulário `app/templates/chamados/partials/modal_fio.html:348` → orquestrador duplicado `app/chamados/services/fio.py:642`.

   **Difere:** A procura disputa aberta somente por `chamado=chamado`; B usa `_disputa_do_chamado`, que também encontra a disputa pela pergunta cujo destino é aquele chamado. Fontes: `app/chamados/views.py:470`, `app/chamados/services/fio.py:564`, `app/chamados/services/fio.py:134`.

   **Converge em `materializar_perguntas_validadas_da_disputa`, `app/chamados/services/materializacao.py:1313`**, chamado pelos dois em `app/chamados/services/acoes_disputa.py:139` e `app/chamados/services/fio.py:666`.

   **Leitor/gravidade: sim, pode mudar dia/dinheiro.** Um caminho pode recusar o chamado do furo que o outro alcança; o escritor materializa batidas/ausências, fecha destinos e fecha a disputa quando quitada. Fontes: `app/chamados/services/materializacao.py:1319`, `app/chamados/services/materializacao.py:1326`, `app/chamados/services/materializacao.py:1308`.

5. **Responder e mudar status/resolver chamado — detalhe/painel × fio/modal × GLPI.**

   **A: D e T**, formulários `app/templates/chamados/detalhe_local.html:385` e `app/templates/chamados/partials/painel_chamado_turno.html:68` → detalhe local → `acao_resposta_livre`, `app/chamados/services/acoes_chamado.py:283`.

   **B: F e M**, formulários `app/templates/chamados/fio_colaborador.html:257` e `app/templates/chamados/partials/modal_fio.html:649` → `processar_post_fio`, `app/chamados/services/fio.py:704`.

   **Difere:** A registra nota `resposta_livre` e logs adicionais `responder`/`status`; B registra `mensagem_dp`. A oferece “Reabrir” como `aberto`; F/M não oferecem essa opção. No HTTP, A aceita status sem texto, B exige texto; os formulários HTML exigem texto nos dois lados. Fontes: `app/chamados/services/acoes_chamado.py:292`, `app/chamados/services/acoes_chamado.py:295`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:705`, `app/chamados/services/fio.py:714`, `app/templates/chamados/detalhe_local.html:398`, `app/templates/chamados/fio_colaborador.html:267`, `app/templates/chamados/detalhe_local.html:387`.

   **O estado converge em `ChamadoColaborador.transicionar`, `app/chamados/models.py:122`, com auditoria canônica comum** em `app/chamados/models.py:274`. Portanto, o fio tem auditoria de estado; faltam nele os logs adicionais citados.

   **C: `/chamados/<ticket_id>/`**, view `app/chamados/views.py:219`, template `app/templates/chamados/detalhe.html:122` → `/chamados/<ticket_id>/followup/`, view `app/chamados/views.py:274` → escritor externo `GLPIClient.adicionar_followup`, `app/chamados/glpi_client.py:219`. Essa tela oferece `novo_status`, mas o handler só envia comentário e não consome esse campo. Fontes: `app/templates/chamados/detalhe.html:130`, `app/chamados/views.py:292`, `app/chamados/views.py:303`.

   **Leitor/gravidade:** timeline e estado/perguntas do chamado veem a diferença; os sinais comuns estão em `app/chamados/signals.py:76`. **Não há escrita direta de dinheiro nesses handlers**; efeito financeiro decorrente dessa diferença: **[não verificado]**. Fontes dos atos: `app/chamados/services/acoes_chamado.py:283`, `app/chamados/services/fio.py:704`, `app/chamados/views.py:303`.

6. **Abrir mensagem para colaborador — formulário completo × mensagem rápida.**

   **A:** `/chamados/abrir/`, view `app/chamados/views.py:147`, template `app/templates/chamados/form_abertura.html:127` → escritor `abrir_chamado_para_colaborador`, `app/chamados/services/abertura.py:122`.

   **B:** `/colaboradores/<cid>/painel/`, view `app/colaboradores/views.py:915`, template `app/templates/colaboradores/partials/painel_colaborador.html:188` → POST `/chamados/colaborador/<cid>/mensagem-rapida/`, view `app/chamados/views.py:961` → `ChamadoColaborador.abrir`, chamado em `app/chamados/views.py:973`.

   **Difere, comparando `mensagem_dp`:** A deduplica pela entrada completa, grava descrição, prazo, autor/contexto e log. B reutiliza mensagem DP viva e acrescenta ocorrência. Fontes: `app/chamados/services/abertura.py:165`, `app/chamados/services/abertura.py:172`, `app/chamados/services/abertura.py:194`, `app/chamados/views.py:973`, `app/chamados/views.py:986`.

   **Leitor/gravidade: não muda dia/dinheiro diretamente.** Muda a composição do fio: chamado próprio com descrição versus ocorrência em chamado reutilizado. Fontes: `app/chamados/services/abertura.py:177`, `app/chamados/views.py:986`, `app/chamados/views.py:997`.

7. **Criar regularização externa — painel de lotes × drawer.**

   **A:** `/chamados/regularizacao/colab/<cid>/`, view `app/chamados/views_regularizacao.py:26`, template `app/templates/chamados/regularizacao_painel.html:14` → POST `.../criar/`, view `app/chamados/views_regularizacao.py:42` → `abrir_lote`, `app/chamados/services/regularizacao_ext.py:309`.

   **B: G**, formulário `app/templates/colaboradores/partials/_form_regularizar.html:2` → `/colaboradores/colab/<cid>/regularizar/`, view `app/colaboradores/views.py:2948` → `criar_regularizacao_externa`, `app/chamados/services/regularizacao_ext.py:230`.

   **Converge em `criar_regularizacao_externa`, mas difere:** A exige motivo textual, abre lote vazio e deduplica esse lote; B exige categoria e itens, permite observação vazia e chama diretamente o criador. A usa gate gestor/departamento supervisão; B usa `tem_acao('regularizar_batida')`. Fontes: `app/chamados/views_regularizacao.py:19`, `app/chamados/views_regularizacao.py:48`, `app/chamados/services/regularizacao_ext.py:312`, `app/colaboradores/views.py:2957`, `app/colaboradores/views.py:2961`, `app/colaboradores/views.py:2981`.

   **Leitor/gravidade:** a categoria altera o agrupamento de métricas; os itens alimentam a materialização confirmada. **Sim, potencialmente muda dia/dinheiro**, porque B aceita itens sem os mesmos controles posteriores de A, incluindo a recusa de entrada=saída. Fontes: `app/chamados/services/regularizacao_ext.py:277`, `app/chamados/views_regularizacao.py:120`, `app/colaboradores/views.py:2976`, `app/chamados/services/regularizacao_ext.py:179`.

8. **Retratar batida — calendário × drawer.**

   **A: C; B: G.** Ambos incluem o formulário em `app/templates/colaboradores/partials/_calendario_grade.html:121` → POST `/ponto/batida/<id>/retratar/`, view `app/ponto/views.py:3274` → **converge em `retratar`, `app/ponto/services/retratar_humano.py:150`**.

   **Difere:** C fornece `motivos_retratacao`; o retorno do contexto de G não fornece essa chave. O formulário exige motivo e o escritor exige cadastro ativo. Fontes: `app/colaboradores/views.py:1958`, `app/colaboradores/services/drawer.py:139`, `app/templates/colaboradores/partials/_calendario_grade.html:121`, `app/ponto/services/retratar_humano.py:166`.

   **Leitor/gravidade: sim, pode mudar dia/dinheiro.** A retratação tira a marcação do universo apurável usado pelo espelho. O impedimento do formulário no drawer é inferido do código; reprodução em navegador: **[não verificado]**. Fontes: `app/ponto/turnos.py:2864`, `app/ponto/services/espelho.py:1022`.

9. **Inverter E/S — fila de flip × calendário/drawer.**

   **A:** `/ponto/flip-fila/`, view `app/ponto/views.py:3299`, template `app/templates/ponto/flip_fila.html:16`.

   **B: C e G**, formulário comum em `app/templates/colaboradores/partials/_calendario_grade.html:121`.

   Todos → POST `/ponto/batida/<id>/flip/`, view `app/ponto/views.py:3260` → **converge em `flip_tipo`, `app/ponto/services/flip_batida.py:10`**.

   **Difere:** a fila envia motivo descritivo; calendário/drawer omitem o campo, aceito pelo escritor. A descrição da trilha fica diferente e o leitor `flips_de` recupera esse motivo. Fontes: `app/templates/ponto/flip_fila.html:16`, `app/templates/colaboradores/partials/_calendario_grade.html:121`, `app/ponto/services/flip_batida.py:31`, `app/ponto/services/flip_batida.py:75`.

   **Gravidade da diferença: não muda dinheiro/dia; muda a explicação auditável.** A troca de tipo e o recompute usam o mesmo caminho. Fontes: `app/ponto/services/flip_batida.py:30`, `app/ponto/services/flip_batida.py:39`.

10. **Cobrar por push — individual × massa.** [nao verificado]

    **A: M e Central**, botões `app/templates/chamados/partials/modal_fio.html:345` e `app/templates/chamados/partials/_lista_chamados.html:84` → `/chamados/cobrar/<cid>/`, view/escrita `app/chamados/views_cobrar.py:24`.

    **B: Central**, template `app/templates/chamados/painel_gestao.html:59` → `/chamados/cobrar/massa/`, view/escrita `app/chamados/views_cobrar.py:61`.

    **Difere:** massa restringe aos vivos da fila “cobrar”, exige `muda_a_folha` e escolhe um chamado por colaborador. Individual parte do chamado solicitado sem esses filtros. Ambos aplicam limite diário por colaborador e gravam ocorrência após envio, com tipos diferentes. Fontes: `app/chamados/views_cobrar.py:29`, `app/chamados/views_cobrar.py:36`, `app/chamados/views_cobrar.py:76`, `app/chamados/views_cobrar.py:95`, `app/chamados/views_cobrar.py:97`, `app/chamados/views_cobrar.py:117`, `app/chamados/views_cobrar.py:132`.

    **Leitor/gravidade: não muda dinheiro/dia diretamente; muda quem recebe cobrança e a nota no fio.** O filtro exclusivo da massa exclui chamados sem data e competência exportada. Fontes: `app/chamados/services/cobranca_folha.py:54`, `app/chamados/services/cobranca_folha.py:69`.

**Dois cruzamentos adicionais de escritores de Batida — separados da contagem dos dez grupos**

| Ações existentes | Caminhos e diferença | Leitor / gravidade |
|---|---|---|
| **Corrigir tipo na regularização × inverter E/S** | A tela `/chamados/regularizacao/<reg>/editar/`, view `app/chamados/views_regularizacao.py:60`, formulário `app/templates/chamados/regularizacao_editar.html:45`, prepara `corrigir`; após confirmação, o escritor altera `tipo` e `timestamp` diretamente em `app/chamados/services/regularizacao_ext.py:153`. Flip, nas telas C/G/fila, passa por `app/ponto/services/flip_batida.py:10` → `anotar`, com guarda de retratada/competência e trilha de flip. Fontes: `app/ponto/services/flip_batida.py:15`, `app/ponto/services/flip_batida.py:21`, `app/ponto/services/flip_batida.py:30`. | **Sim, pode mudar dia/dinheiro:** tipo/hora alteram o pareamento; a trilha específica de flip também deixa de ser a mesma. Leitores: `app/ponto/services/espelho.py:1022`, `app/ponto/services/flip_batida.py:66`. |
| **Remover na regularização × retratar marcação** | A mesma tela oferece `remover`, `app/templates/chamados/regularizacao_editar.html:51`; após confirmação, executa `alvo.delete()`, `app/chamados/services/regularizacao_ext.py:136`. C/G retratam preservando a Batida, por `app/ponto/registro_batida.py:267`. O desfazer humano só reconhece retratação da sua própria porta, `app/ponto/services/retratar_humano.py:199`. | **Sim, muda o fato disponível e a reversibilidade:** exclusão versus registro preservado fora da apuração. O universo apurável filtra retratação em `app/ponto/t

[truncado em 20000 de 24883 caracteres]

## 06/11 21:26 · sessao 82a55b25c563 · FIO-COM-ASSENTO-E0-V3 · leitura · origem=Code

| Pergunta | Resultado da conferência |
|---|---|
| **(1) Juízes** | Os endereços existem, mas respondem perguntas diferentes: acusação do **dia**, sustentação de **classe de chamado**, cobertura de **ausência**, geometria de **turno** e oferta de **ação na tela**. Não formam, por si, uma autoridade de exigibilidade por motivo. `app/escala/models.py:1767`; `app/chamados/catalogo/premissa.py:144`; `app/colaboradores/services/calendario.py:97`. |
| **(2) Censo** | Há portas de escrita já centralizadas por campo/entidade, mas leitores têm universos diferentes. Pergunta não grava `status`; disputa também não: usam, respectivamente, estado derivado e `fechada_em`. `app/chamados/models.py:2492`; `app/chamados/models.py:1763`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:36`. |
| **(3) Contradições** | A1.2 reaproveita qualquer estado; identidade não inclui episódio; correção de dia preenchido não é re-chaveamento; há ressuscitação explícita. Outbox e preservação de respostas já existem, com contratos diferentes do v3. `app/chamados/models.py:2398`; `app/chamados/services/fio.py:832`; `app/chamados/models.py:1461`; `app/chamados/services/disputa_emissao.py:1393`. |
| **(4) Exigibilidade** | **Não achei** função equivalente à pergunta completa. As aproximações trabalham sobre pergunta existente, chamado existente ou célula, sem o mesmo universo. `app/chamados/juizes.py:822`; `app/chamados/juizes.py:1886`; `app/chamados/juizes.py:1341`. |
| **(5) Cortes e faltas** | Retiraria a apresentação de mecanismos existentes como novidade; reformularia “PM não julga” e S4. Faltam contrato de universo, desconhecido, veto, horizonte, colisão de identidade e distinção entre porta, permissão e entrega. `CLAUDE.md:189`; `CLAUDE.md:275`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:145`; `app/ponto/services/furos_vetados.py:27`. |

[Não verificado] Incidência e contagens em produção, execução dos selos e autenticação do HASH como HEAD: não houve acesso a banco/log; o anexo contém o lookup, sem ocorrências; `git rev-parse HEAD` falhou nesta cópia por ausência de repositório Git utilizável. A análise abaixo é estática.

**(1) Endereço, assinatura, pergunta e universo dos juízes**

| Item citado | Assinatura e endereço | O que responde de fato; limite para o v3 |
|---|---|---|
| `VEREDITOS_ACUSAM` | **Constante**, sem assinatura: `CelulaDia.VEREDITOS_ACUSAM`. `app/escala/models.py:1767`. | Pertencimento do veredito do **dia** a `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu`, `fato_em_ausencia`. Não discrimina motivo/marco: a pergunta pode morrer pelo seu marco aceso enquanto outro marco mantém o dia acusando. `app/escala/models.py:1767`; `app/chamados/juizes.py:1173`. |
| `premissa_morta` | `(modulo_origem, subcategoria=None, fatos: Optional[Mapping]=None) -> bool`. `app/chamados/catalogo/premissa.py:144`. | Classe conhecida cujo fato sustentador foi informado como falso. Classe desconhecida ou fato não coletado **não** produz premissa morta. Recebe fatos do coletor; não consulta colaborador, dia ou banco. Seu complemento lógico não prova exigibilidade. `app/chamados/catalogo/premissa.py:115`; `app/chamados/catalogo/premissa.py:124`; `app/chamados/catalogo/premissa.py:149`. |
| `ausencia_cobre` | `(colab, data_ref, *, para)`. `app/ponto/turnos.py:2536`. | Retorna **uma ausência vencedora**, por maior data/PK, que cobre a data. Para cobrança exclui rejeitadas; para folha aceita somente aprovadas. Não responde sozinho se cobre o dia inteiro ou o marco cobrado. `app/ponto/turnos.py:2509`; `app/ponto/turnos.py:2542`; `app/ponto/turnos.py:2552`. |
| `dia_sem_trabalho` | `(cel) -> bool`. `app/chamados/juizes.py:413`. | Cala por `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de trabalho. Na ausência parcial, só `trabalha=False` cala. `None` não equivale a folga. Não é o previsto calculado da escala. `app/chamados/juizes.py:418`; `app/chamados/juizes.py:422`. |
| `STATUS_QUE_PEDEM_DECISAO` | **Tupla**: `('falta', 'irregular', 'aberto', 'sem_certidao')`. O predicado é `resolver_dia_oferecido(dia, hoje)`. `app/colaboradores/services/calendario.py:94`; `app/colaboradores/services/calendario.py:97`. | Oferta do botão “Resolver dia”: **último recurso + passado + status**. A permissão do usuário fica fora. Usar apenas a tupla perde duas condições; usar o resultado como prova de responsável alcançável acrescenta uma condição que ele não verifica. `app/colaboradores/services/calendario.py:100`. |
| “Cobertura parcial” | Família: `cobre_parte_do_dia(a, parciais=None)`; `ausencia_explica_o_dia_inteiro(a, parciais=None)`; `periodo_da_parcial_no_dia(a, horas)`; `ausencia_parcial_cobra_o_resto(registro)`. `app/ponto/turnos.py:2583`; `app/ponto/turnos.py:2668`; `app/ponto/turnos.py:2614`; `app/chamados/juizes.py:400`. | São perguntas distintas: parcialidade do registro; explicação integral; relação com período declarado; leitura do carimbo da grade/ata. A parcialidade considera tipo que aceita horas, um dia e minutos abaixo de 480; não basta a flag do tipo. `app/ponto/turnos.py:2565`; `app/ponto/turnos.py:2575`; `app/chamados/juizes.py:410`. |
| “Juiz do turno” | Autoridade registrada: `turnos_do_colab(colab, ini, fim, celulas=None)`. Corpo do dia: `_data_do_turno(entrada, vespera, saida=None, cont_max_s=14*3600)`. `app/core/juizes.py:196`; `app/ponto/turnos.py:1848`; `app/ponto/turnos.py:246`. | Geometria de turnos a partir de batidas/marcos, com tratamento da véspera e cruzamento de meia-noite. Não oferece sozinho dia canônico para toda ocorrência sem batida/turno. `app/ponto/turnos.py:247`; `app/ponto/turnos.py:1860`. |

Há formas de cobertura com universos diferentes: `cobertura_ausencia_periodo(colab, ini, fim, *, para)` responde o período; `ausencias_do_periodo(...)` entrega **todas** as ausências que o tocam, sem o desempate da ausência vencedora. Não são duplicatas intercambiáveis de `ausencia_cobre`. `app/ponto/turnos.py:2733`; `app/ponto/turnos.py:2521`.

O dia do **chamado** tem outras autoridades: `data_do_chamado(modulo_origem, contexto_json)` lê a chave declarada do catálogo; `dia_do_fato(chamado, fortes=False)` prioriza célula, catálogo e `data_turno`, acrescentando fontes fracas quando permitido. Nenhuma dessas funções canonicaliza o chamado executando o pareador. `app/chamados/catalogo/modulos.py:219`; `app/chamados/juizes.py:80`; `app/chamados/juizes.py:120`.

A comparação com o anexo confirma a separação entre geometria, cobertura e dia do chamado. Confirma também a exceção parcial e a divergência declarada de `_protesta`: ele continua lendo somente `trabalha is not False` e marcos acesos, sem a segunda cláusula de `dia_sem_trabalho`. `app/docs/JUIZES.md:20`; `app/docs/JUIZES.md:50`; `app/docs/JUIZES.md:56`; `app/docs/JUIZES.md:59`; `app/docs/JUIZES.md:70`; `app/ponto/services/cartorio.py:1407`.

Não achei `STATUS_QUE_PEDEM_DECISAO` ou `resolver_dia_oferecido` na tabela de `app/docs/JUIZES.md:20` até seu encerramento em `app/docs/JUIZES.md:107`. O próprio documento se declara lookup, subordinado ao registro vivo e ao código. `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`.

**(2) Censo por fronteira de decisão**

O censo abaixo distingue portas que atribuem os campos, operações que as acionam e consumidores. A exclusão absoluta de escritores dinâmicos e o resultado atual dos testes ficam [não verificado]; o selo AST declara limites para receptores genéricos e argumentos não resolvidos. `app/chamados/tests/test_selo_status_so_pelas_portas.py:15`.

| Fronteira | Objeto | Escritores, operações e leitores |
|---|---|---|
| **Escrita** | Identidade, `data_evento`, `chamado_destino` | `PerguntaDisputa.abrir`: nascimento, dedup e reapontamento; `fio.reapontar_resolvedora`: destino e preenchimento de data ausente, com relavra no mesmo save. O contrato enumera **dois módulos** para esses campos. `app/chamados/models.py:2398`; `app/chamados/services/fio.py:784`; `app/core/tests/test_contract_escritores_estado.py:48`. |
| **Escrita** | Resposta e validação da pergunta | Resposta humana em `responder_pergunta_disputa_core`; resposta automática e reconstrução do instante em portas próprias. Validação/devolução em `validacao`; reabertura para correção preserva resposta anterior e limpa os campos atuais. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1422`; `app/chamados/services/disputa_emissao.py:1439`; `app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:187`; `app/chamados/services/disputa_emissao.py:1393`. |
| **Escrita** | Materialização/via da pergunta | Núcleo `_escrever_carimbo`; portas `carimbar`, `carimbar_identidade`, `descarimbar`, `descarimbar_em_lote`; operações próprias de encerramento/reabertura por via. `app/chamados/models.py:2556`; `app/chamados/models.py:2562`; `app/chamados/models.py:2625`; `app/chamados/models.py:2646`; `app/chamados/models.py:2692`; `app/chamados/models.py:2702`. |
| **Escrita** | Estado do chamado | `_gravar_estado` centraliza mudança persistida; `abrir` e `criar` escrevem nascimento. `_vivas_entre` altera somente memória para julgamento do pós-estado. A dispersão dos chamadores não significa escritores livres de `status_local`. `app/chamados/models.py:195`; `app/chamados/models.py:796`; `app/chamados/models.py:1015`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:36`. |
| **Escrita** | Abertura/fechamento da disputa | `fechar`, `reabrir_admin`, `reabrir_sistema`; fechamento grava `fechada_em`, autor, resolução e motivo em transação. Não há coluna `status` equivalente à do chamado. `app/chamados/models.py:1796`; `app/chamados/models.py:1845`; `app/chamados/models.py:2040`; `app/chamados/models.py:2077`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:47`. |
| **Escrita** | Célula consumida pela cobrança | Geração, regeneração, edição/reversão do DNA; `lavrar_veredito` para ata/veredito/via/instante/protestos/status; `carimbar` para impressão, ausência e presença de entrada/saída. `app/ponto/portas/celula.py:110`; `app/ponto/portas/celula.py:534`; `app/ponto/portas/celula.py:740`; `app/ponto/portas/celula.py:811`; `app/ponto/portas/celula.py:969`; `app/ponto/portas/celula.py:1047`. |
| **Materialização** | Pergunta → fato e fechamento | `materializar_perguntas_validadas_da_disputa` seleciona respondidas, validadas e não materializadas; interpreta hora/data-hora/escolha, usa destino e fecha chamados/clones. Divergência de data bloqueia até conferência. `app/chamados/services/materializacao.py:1313`; `app/chamados/services/materializacao.py:1344`; `app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:1156`. |
| **Materialização** | Cache do julgamento | `veredito_lavrado`, `julgado_em`, `janela_expira_em` são escritos por `lavrar`; existe também comando de backfill declarado como escritor. `app/chamados/veredito_lavrado.py:211`; `app/core/tests/test_contract_escritores_estado.py:63`. |
| **Consulta** | Pergunta, identidade e disputa | `do_fato`; C6 `pergunta_viva`; `.estado` e `.viva`; `q_vivas` como pré-filtro generoso; `DisputaSupervisao.pode_fechar`. O SQL de `q_vivas` não substitui C6. `app/chamados/models.py:2381`; `app/chamados/juizes.py:822`; `app/chamados/models.py:2492`; `app/chamados/models.py:2770`; `app/chamados/models.py:2798`; `app/chamados/models.py:1767`. |
| **Consulta** | Cobrança | `aviso_pendencia` seleciona C6 vivas sem resposta; `cobranca` lê dias, disputas e prazo do pai; fábrica lê célula/ata, situação, teto e histórico; reconciliação lê premissa, célula e lastro. `app/chamados/services/aviso_pendencia.py:7`; `app/chamados/services/cobranca.py:45`; `app/chamados/services/cobranca.py:75`; `app/chamados/services/fabrica_da_celula.py:74`; `app/chamados/reconciliador.py:1353`. |
| **Consulta** | Marcos/cobertura da célula | Leitores incluem `_marcos_faltantes`, `_coberto_por_ausencia`, `pergunta_viva`, cartório e vigia. Leem `trabalha`, data, ata/tipo do dia, cobertura/período parcial, lâmpadas, veredito/via e carimbos. `app/chamados/services/declaracao_texto.py:65`; `app/chamados/services/materializacao.py:55`; `app/chamados/juizes.py:1135`; `app/ponto/services/cartorio.py:1413`; `app/chamados/services/vigia_fio.py:378`. |
| **Tela admin** | Pergunta/destino/disputa | Fio ancora no destino; inclui perguntas C6 vivas em disputa fechada. Modal mostra dias divergentes, conferência, resposta e carimbos. Relatório de disputas restringe-se às abertas. `app/chamados/services/fio.py:97`; `app/chamados/services/fio.py:209`; `app/templates/chamados/partials/modal_fio.html:156`; `app/templates/chamados/partials/modal_fio.html:212`; `app/chamados/relatorios_disputa.py:11`. |
| **Tela app** | Pergunta/destino/estado | API usa `perguntas_do_app`, sem filtro de disputa aberta; publica estado, espera pelo admin e destino. POST exige C6 viva e chama a porta de resposta. `app/chamados/services/pergunta_no_app.py:42`; `app/api/views.py:2551`; `app/api/views.py:2568`; `app/api/views.py:2627`. |
| **Tela PWA/badge** | Pergunta/disputa | Fio do colaborador com `so_janela=True` segue pelas disputas abertas; badge também exige disputa aberta. Card apresenta data/resposta e condiciona formulário por `.viva`. `app/chamados/services/fio.py:209`; `app/chamados/models.py:1729`; `app/templates/chamados/partials/_card_atendimento.html:77`. |

**Os significados não são uniformes em todos os leitores:**

| Campo/conceito | Unidade e divergência |
|---|---|
| `PerguntaDisputa.estado` | É derivado. Pode devolver pendente/respondida mesmo quando C6 considera a pergunta morta sem carimbo. Portanto, “pendente” não prova exigibilidade. `app/chamados/models.py:2492`; `app/chamados/models.py:2502`; `app/chamados/models.py:2802`. |
| `data_evento` | É `DateField`, mas pode nascer do contexto/grade, da **data civil local da batida** ou do **dia civil do nascimento da conversa**. Não é universalmente turno canônico. `app/chamados/models.py:2218`; `app/chamados/services/disputa_emissao.py:319`; `app/chamados/services/disputa_emissao.py:350`; `app/chamados/models.py:2422`. |
| `chamado_destino` | É resolvedor e âncora de apresentação; o prazo da cobrança fica no **pai da disputa**. Destino não equivale a responsável nem a portador do SLA. `app/chamados/services/fio.py:97`; `app/chamados/services/cobranca.py:51`. |
| Célula | `trabalha` pertence ao previsto; ata/veredito ao julgamento; pergunta cobra um marco. A acusação do dia não identifica automaticamente o motivo exigível. `CLAUDE.md:167`; `app/chamados/juizes.py:1173`. |
| Carimbos | `respondida_em`, `validada_em`, `materializada_em` são instantes; `data_evento` é dia. O contrato exige timestamps em UTC e apresentação local. `app/chamados/models.py:2218`; `app/chamados/models.py:2228`; `app/chamados/models.py:2827`; `CLAUDE.md:143`. |
| Abertura da disputa | Significa `fechada_em is None`; não determina sozinha a vida da pergunta. API/admin podem ler viva em disputa fechada, enquanto badge/PWA restringem as abertas. `app/chamados/models.py:1763`; `app/chamados/services/pergunta_no_app.py:46`; `app/chamados/services/fio.py:209`; `app/chamados/models.py:1736`. |

O `vigia_fio` também tem fronteiras próprias:

- **Consulta de domínio:** `censo` parte de chamados `VIVOS` com célula; `censo_alcance` parte de dias que seguram a folha, por empresas visíveis e competências a lavrar. `dono_fora_do_fio` compartilha a composição dos baldes legais. `app/chamados/services/vigia_fio.py:334`; `app/chamados/services/vigia_fio.py:542`; `app/chamados/services/vigia_fio.py:577`; `app/chamados/services/vigia_fio.py:274`.
- **Materialização de medição:** o command escreve carimbo de frota com censo/alcance/passivos; recorte por empresa não substitui esse carimbo. Isso é escrita de instrumento, não transição de obrigação. `app/chamados/management/commands/vigiar_fio_morto.py:115`; `app/chamados/management/commands/vigiar_fio_morto.py:210`.
- **Execução/leitura:** cron chama o command; leitor host consome o carimbo e recusa universo vazio como verde. `app/config/crons.py:1703`; `bin/fio_estado.py:65`; `bin/fio_estado.py:74`.

**(3) Onde o comportamento atual difere do v3**

| Ponto | Conferência |
|---|---|
| **A1.2 em qualquer estado** | **Confirmado.** `do_fato` filtra somente colaborador, motivo e data; `abrir` pega a última linha e a devolve. Pode trocar célula/destino e relavrar, mas o gate não limpa resposta/carimbos nem reabre por esse reaproveitamento. `app/chamados/models.py:2392`; `app/chamados/models.py:2431`; `app/chamados/models.py:2465`. |
| **Identidade + episódio** | A identidade atual não contém episódio. “Episódio” já designa agrupamento pela mesma batida-origem e clones relacionados, outro significado. `app/chamados/models.py:2400`; `app/chamados/services/disputa_emissao.py:1018`; `app/chamados/models.py:2838`. |
| **Correção do dia re-chaveia a mesma ocorrência** | A porta de costura só preenche data antes vazia. Data diferente entregue ao nascimento procura outra chave. **Não achei** protocolo de re-chaveamento/colisão nesses dois caminhos. `app/chamados/services/fio.py:831`; `app/chamados/models.py:2431`. |
| **Extinção/encerramento** | Hoje há encerrados reabríveis, vias de resolução reversíveis e descarimbo. Não existe equivalência geral entre “resolvida”, “materializada”, “disputa fechada” e extinção do v3. `app/chamados/catalogo/motor.py:30`; `app/chamados/models.py:1323`; `app/chamados/models.py:2723`. |
| **Quem ressuscita** | `reabrir` chama `ressuscitar_perguntas`; cartório também a chama. A porta testa marco e C6 sobre cópia no pós-estado. Existem ainda inversas por fantasma, resposta e competência, além de relavra sem lastro. `app/chamados/models.py:1402`; `app/ponto/services/cartorio.py:1622`; `app/chamados/models.py:1420`; `app/chamados/models.py:2723`; `app/chamados/services/relavra_via.py:217`. |
| **Guarda contra renascimento** | Já existe proteção para fechamento administrativo sem fato novo, premissa regularizada, agrupamento e irmão vivo. Ela reutiliza a linha atual; não cria episódio pós-extinção. `app/chamados/models.py:1296`; `app/chamados/models.py:1309`; `app/chamados/models.py:1314`; `app/chamados/models.py:1390`. |
| **Suspensa sem cobrança** | Silêncio atual é véu, sem mudar status/apuração. Fila admin exclui silêncio futuro; seleção do app não consulta esse véu. `recobrar_questionario` conta perguntas cruas sem resposta e envia push sem guarda de silêncio nesse corpo. Ocorrência real de recobrança durante suspensão: **[não verificado]**. `app/chamados/services/arquivo.py:10`; `app/chamados/services/painel.py:186`; `app/chamados/services/pergunta_no_app.py:42`; `app/chamados/services/disputa_emissao.py:1485`. |
| **Responsável inalcançável** | Há juiz de canal de push, mas isso não prova acesso humano. A cobrança abre prazo mesmo se entrega falhar e orienta o admin a alcançar pelo supervisor; outro emissor ignora os retornos dos drivers. **Não achei** conversão automática para `incident` nesses caminhos. `app/core/canal.py:19`; `app/chamados/services/cobranca.py:173`; `app/chamados/services/cobranca.py:206`; `app/chamados/services/disputa_emissao.py:1540`. |
| **Exatamente um encaminhamento** | A autoridade de alcance admite várias portas/verbos simultâneos e consolida um `fio` por precedência. Unicidade de responsabilidade consolidada e unicidade de porta são propriedades diferentes. `app/chamados/juizes.py:1323`; `app/chamados/juizes.py:1350`; `app/chamados/juizes.py:1386`. |
| **Único módulo de transições** | Hoje há portas distintas para chamado, carimbo, resposta, validação, silêncio e costura. Isso difere de um PM único, mas não prova escrita descontrolada: vários campos já têm escritor canônico. `app/chamados/tests/test_selo_status_so_pelas_portas.py:36`; `app/core/tests/test_contract_escritores_estado.py:35`; `app/core/tests/test_contract_escritores_estado.py:83`. |

O outbox já cobre lavra da célula e alterações relevantes da pergunta, incluindo decisão administrativa pelos campos o

[truncado em 20000 de 29988 caracteres]

## 06/11 21:26 · sessao d5f797084ef3 · - · - · origem=Ronald (tmux)

Ampliação fechada: grupo **trocar vínculo de escala** passa a ter 3 telas e 3 pares: manual–lista, manual–modal e lista–modal; todas convergem em `julgar_vinculo` / `executar_vinculo` (`app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`, `app/escala/regua_defesa.py:316`, `app/escala/regua_defesa.py:321`).

Tela manual: GET `/colaboradores/vincular/<cid>/painel/` → `painel_vinculo`, POST `/colaboradores/<cid>/vincular/escala/` → `vincular_escala`; formulário `colaboradores/partials/painel_vinculo.html` (`app/colaboradores/urls.py:12`, `app/colaboradores/urls.py:13`, `app/colaboradores/views.py:262`, `app/colaboradores/views.py:373`, `app/templates/colaboradores/partials/painel_vinculo.html:136`).

Os dois novos pares diferem porque manual recebe template, início e âncora humanos; as duas telas sugeridas fixam hoje e recusam ciclo antes de chamar o juiz. Manual permite retroagir até a janela anterior e criar ciclo com âncora declarada (`app/templates/colaboradores/partials/painel_vinculo.html:155`, `app/templates/colaboradores/partials/painel_vinculo.html:159`, `app/templates/colaboradores/partials/painel_vinculo.html:167`, `app/escala/regua_defesa.py:310`, `app/escala/regua_defesa.py:313`, `app/colaboradores/services/vinculo.py:69`, `app/colaboradores/services/vinculo.py:126`).

**Muda dia/dinheiro? Sim, potencial:** o início escolhido determina o trecho regenerado até hoje; regeneração padrão recalcula por evento as competências tocadas. Ocorrência efetiva em produção [não verificado] (`app/colaboradores/services/vinculo.py:320`, `app/ponto/portas/celula.py:110`, `app/ponto/portas/celula.py:354`, `app/ponto/portas/celula.py:445`, `app/ponto/portas/celula.py:458`). Leitor do dia: `eh_dia_trabalho` retorna `CelulaDia.trabalha` (`app/escala/models.py:1094`).

Copiar tipo GET versus criar pelo wizard **não recomendo contar como par**: os comandos de tela são explicitamente “Copiar esta escala” e “Nova escala”, e não há segunda tela comprovada para o mesmo comando copiar (`app/templates/escala/tipos_lista.html:90`, `app/escala/views.py:108`). A cópia parcial existe: `copiar_tipo` cria com subconjunto explícito e não inclui calendário padrão, override por dia, HE fixa e duração do intervalo; isso é diferença entre comandos distintos (`app/escala/services/cadastro_tipo.py:367`, `app/escala/models.py:89`, `app/escala/models.py:203`, `app/escala/models.py:78`, `app/escala/models.py:110`).

## 06/11 21:26 · sessao 2f360630892b · - · - · origem=Ronald (tmux)

Confirmado: **não contar “recalcular” × “gerar TXT” como par da mesma ação**. Recalcular persiste o fechamento (`app/ponto/views.py:1252`); gerar TXT solicita `somente_leitura=True` (`app/folha/views.py:208`), cujo ramo entrega objetos sem salvar (`app/ponto/services/fechamento.py:1121`), depois aprova e registra exportação (`app/folha/views.py:252`, `:297`).

Correção ao inventário de telas: aprovação **selecionada** está acessível pelo JavaScript em `app/templates/ponto/fechamento.html:143`. Aprovação **cega**, recalcular antigo e reabrir competência estão dentro do bloco `display:none` aberto em `:152`, com formulários em `:209`, `:189`, `:223`, fechado em `:234`. O recalcular visível está em `:87`. Portanto, apresentar o cego como **endpoint existente com formulário oculto**, sem presumir botão acessível.

Reabrir individual e reavaliar vazios provêm da **mesma tela de fechamento**: `app/templates/ponto/_fech_linha.html:42` e `app/templates/ponto/fechamento.html:76`. A linha é incluída tanto na página (`:262`) quanto no fragmento de paginação (`app/templates/ponto/_fech_lote.html:8`), sem segunda experiência de tela independente comprovada.

Leitor relevante da diferença de carimbo: `competencia_fechada` consulta fechamento da pessoa e recusa `aprovado/exportado` (`app/ponto/janelas.py:86`, `:87`). O registro institucional de batida recusa antes de criar (`app/ponto/registro_batida.py:132`, `:137`); flip recusa antes de alterar (`app/ponto/services/flip_batida.py:21`, `:24`). Isso altera a possibilidade de regularizar posteriormente; mudança efetiva de dinheiro/dia por tentativa posterior é **[não verificado]**. O ato de aprovação isolado só grava carimbo/observação (`app/ponto/services/fechamento.py:1322`, `:1330`).

## 06/11 21:25 · sessao 4ab21ee7a095 · BUG-PLANTIO-NOTURNO-MAIS-1 · censo · origem=Code

**Não atribuo descarte à triagem sem evidência da decisão.** A regra exige registrar o motivo do descarte; “foi visto” e “foi descartado” precisam ser demonstrados separadamente. (`CLAUDE.md:368`, `CLAUDE.md:377`)

Os trechos abaixo são transcrições dos anexos fornecidos; sua numeração física em disco é [nao verificado].

1. **(a) — falha do Codex no núcleo “troca de significado sem backfill”, com percepção parcial do passivo.**

   **Pergunta exata — v1.q, item 6:**
   > pouso antes do pre-requisito; campo com dois significados. [nao verificado]

   **Pergunta exata — f1.q:**
   > quantas perguntas VIVAS (aberta/respondida) com D_e_dia_da_saida -- essas sao as que mudam de leitura quando o leitor migrar

   **Resposta exata — v1.r, item 6:**
   > Suite, pouso e saneamento do passivo: **[nao verificado]**. A costura não converte datas já preenchidas, pois só data perguntas sem dia. [nao verificado]

   **Resposta exata — f1.r:**
   > **(d) VIVAS em `D_e_dia_da_saida`: 81**

   O tema estava perguntado, mas a resposta não denunciou a incompatibilidade entre migrar os leitores e conservar as datas antigas. A costura realmente só preenche perguntas sem data. (`app/chamados/services/fio.py:832`)

   **Triagem:** existe a decisão “passivo so depois da cura no ar”, mas ela não prova descarte de um alerta explícito do Codex sobre essa incompatibilidade. (`app/docs/RELATO.md:41`) Os recortes “sem marcos” e “plantio em D−1” não foram medidos pela saída fornecida. [nao verificado]

2. **(a) — falha do Codex: reconheceu a retirada do recuo, mas não a regressão sobre perguntas antigas.**

   **Pergunta exata — v1.q, item 1:**
   > Ha algum par escritor/leitor em que os significados divergem (ESMERIL-SEMANTICA: unidade, fuso, dia do turno x dia civil x instante)?

   **Resposta exata — v1.r, item 6:**
   > Há asserts que atingem o defeito principal: escritor retorna D, saída planta em D+1, detector grava D e sem marcos não recua.

   A resposta tratou “sem marcos não recua” como comportamento correto, sem separar perguntas novas das antigas, cujo D ainda significava dia da saída. Na cópia, o ramo antigo recua a entrada quando ela ultrapassa a hora da saída. (`app/chamados/juizes.py:315`, `app/chamados/juizes.py:320`)

   **Triagem específica desse risco:** [nao verificado]. As 21 vivas são um teto do recorte SSE fornecido; quantas efetivamente estão sem marcos permanece [nao verificado].

3. **(a) — falha do Codex: inventariou os leitores, mas perdeu o efeito da retirada de D−1 sobre plantios existentes.**

   **Pergunta exata — v1.q, item 1:**
   > Para cada leitor: com que significado ele le (dia do turno, dia civil da batida, dia da saida, dia em que a pergunta foi feita).

   **Resposta exata — v1.r, inventário:**
   > `_plantio_na_hora_do_slot` usa D/D+1

   > `_pergunta_materializada_de_fato`, `_pergunta_viva_calc`

   A resposta mencionou os componentes, mas não apontou a cadeia de perda de reconhecimento → pergunta viva → descarimbo. A cópia mostra a busca antiga incluindo D−1, seu uso como prova de materialização e o descarimbo condicionado a `pergunta_viva`. (`app/chamados/juizes.py:265`, `app/chamados/juizes.py:724`, `app/chamados/services/relavra_via.py:227`, `app/chamados/services/relavra_via.py:231`)

   A fábrica chama esse descarimbo durante a emissão pelo dia. (`app/chamados/services/fabrica_da_celula.py:136`) **Triagem específica:** [nao verificado].

4. **(c) — RED histórico fora do alcance demonstrado; não há certificação do defeito no main.**

   **Pergunta exata — v3.q, item 4:**
   > O RED test_MORDE_saida_sem_dia_do_turno_nao_vira_500 morde o pai (39324b76)?

   **Resposta exata — v3.r, item 4:**
   > **Há mordida dedutiva contra o pai descrito no anexo; execução contra `39324b76`: [nao verificado].**

   **Resposta exata — v4.r, item 3:**
   > objeto e execução contra `39324b76`: **[nao verificado]**.

   O alvo explícito dessas perguntas era o pai intermediário. A resposta não afirmou execução nem prova contra o main original. O relato registra a correção dos “500 nos gemeos” como procedente, mas não demonstra sua existência no main. (`app/docs/RELATO.md:11`) A atribuição histórica aos commits indicados pelo opus permanece [nao verificado].

5. **VISTO; (b) comprovado apenas parcialmente, sem fechar a imputação do núcleo inteiro.**

   **Pergunta exata — v2.q, item 1:**
   > agora deixa de nascer pergunta e some sem rastro alem do log?

   **Pergunta exata — v2.q, item 2:**
   > Ha outro emissor (varredura, cron) que a pega depois, ou ela fica invisivel?

   **Resposta exata — v2.r, item 1:**
   > A recusa específica, entretanto, fica apenas no `logger.info`, sem pergunta ou pendência própria.

   **Resposta exata — v2.r, item 2:**
   > A guarda então pula a disputa e a triagem pode chegar à indicação de sucesso, sem registrar pendência própria nesse ramo.

   Portanto, o silêncio foi explicitamente visto. [nao verificado quanto às linhas físicas dos anexos]

   **Triagem localizada:** o efeito relacionado “push sem pergunta” foi colocado em fila. (`app/docs/BACKLOG.md:1798`) A linha que descarta especificamente a ausência de pendência, prazo, tripwire ou contador é [nao verificado]. Não há base para chamar esse núcleo de falha do Codex nem para fechar integralmente **(b)**.

6. **(a) parcial — o lote `dias_do_fato` não foi apontado; a derivação civil singular foi parcialmente vista.**

   **Pergunta exata — v1.q, item 7:**
   > Ha fallback disfarcado ("se nao achar, usa o dia do timestamp", `or data_entrada`, default de dia) restante em algum escritor tocado ou vizinho?

   **Resposta exata — v1.r, item 7:**
   > O derivador aceita o dia do chamado; esse juiz tem degraus de dia civil da batida e timestamp cru do contexto.

   A resposta reconheceu a família “dia civil da batida”, mas não identificou a duplicação no lote. A cópia confirma ambas as derivações por `localdate(timestamp)`. (`app/chamados/juizes.py:50`, `app/chamados/juizes.py:193`)

   **Triagem exata — v2.q:**
   > FILA: _dia_do_contexto_fraco (juizes.py)

   Essa linha trata do contexto fraco; não demonstra descarte específico de `_dia_da_batida` e `dias_do_fato`. Esse descarte é [nao verificado].

7. **Núcleo VISTO; (b) não comprovado. (a) parcial quanto ao comentário antigo.**

   **Pergunta exata — v1.q, item 6:**
   > derivacao paralela de fonte unica (ex.: dia do turno derivado sem _T1); selo/RED sem mordida

   **Resposta exata — v1.r, item 6:**
   > o construtor de célula apenas devolve a data recebida.

   > `NewType` não estabelece uma validação semântica nessa porta.

   > Os testes novos verificam ausência/`None`, não proveniência do dia. [nao verificado]

   O núcleo do achado — cast sem validação de significado e selo insuficiente para provar proveniência — já estava explicitamente na resposta. [nao verificado quanto às linhas físicas dos anexos]

   **Triagem exata — v2.q:**
   > mypy so checa chamador anotado, selo nao segue **dict/setattr por variavel/SQL cru

   Essa fila cobre limitações relacionadas, mas não comprova descarte do alerta semântico específico. **(b) integral:** [nao verificado]. O comentário que ainda promete “data correta da SAIDA” permanece na cópia e não foi denunciado especificamente nas respostas. (`app/escala/utils.py:913`)

Regras preventivas propostas para as classes identificadas:

- **(a):** Toda troca semântica deve percorrer uma pergunta antiga até seus leitores e efeitos de escrita, incluindo reconhecimento e descarimbo. [nao verificado]
- **(b), parcial:** Cada alerta deve receber triagem própria com decisão, motivo e cura ou adiamento identificável; uma fila vizinha não substitui essa linha. (`CLAUDE.md:368`)
- **(c):** Toda alegação de RED histórico deve identificar o hash original e evidenciar falha pelo motivo correto nesse hash. [nao verificado]

**Não atribuo (d):** a regra local reserva MODO-COMUM ao que Codex **e** opus deixaram passar; os sete achados foram apresentados pelo opus. (`CLAUDE.md:385`; anexo `opus_final.txt` [nao verificado quanto às linhas físicas])

## 06/11 21:24 · sessao 53ecda1d422f · - · - · origem=Ronald (tmux)

Complemento folga manual, com correção do disparador:

- **Plano → aplicar lote:** `/escala/plano-folgas/` → `/escala/plano-folgas/aplicar/`, `app/escala/urls.py:8`, `:10`; view `app/escala/views_plano_folgas.py:144` chama `aplicar_folgas_em_lote` em `:185`; template dispara POST em `app/templates/escala/plano_folgas.html:171`; escritor `app/escala/folgas.py:362` → `_foto_do_dia` cria em `:337`, altera em `:342` ou apaga em `:333`.
- **Detalhe → aceitar `FOLGA_CONTESTA`:** `/chamados/local/<id>/`, `app/chamados/urls.py:14`; view `app/chamados/views.py:359`, ramo `:483`, chamada `:501`; template `app/templates/chamados/detalhe_local.html:310` inclui `app/templates/chamados/partials/_pergunta_card.html:92`, POST com ação em `:95`.
- **Modal → aceitar:** `/chamados/modal/<colaborador>/`, `app/chamados/urls.py:39`; view `app/chamados/views.py:869` delega em `:954`; template `app/templates/chamados/partials/modal_fio.html:274`, ação `:276`.
- **Inline → aceitar:** `/chamados/validar-inline/<cid>/`, `app/chamados/urls.py:33`; view `app/chamados/views_cobrar.py:145` chama em `:208`; botão `app/templates/chamados/partials/_lista_chamados.html:82`.

Os três aceites convergem em `app/chamados/services/acoes_disputa.py:19` → `validar_pergunta` reconhece folga em `app/chamados/services/validacao.py:382` → `_aceitar_a_folga` chama `declarar_dia` em `:291` → `app/ponto/services/declarar_dia.py:189` → escritor `lancar_folga`, `app/escala/folgas.py:280`, `:312`.

**Um grupo Plano × aceites, três pares atômicos divergentes:**

1. Plano recusa qualquer dia anterior ao início da competência atual: `app/escala/folgas.py:379`, `:385`. Aceite permite dia com até 120 dias de antiguidade: constante `app/ponto/services/declarar_dia.py:53`, verificação `:101`.
2. Plano aceita tipo escolhido `F1…F9/FD`, `app/escala/views_plano_folgas.py:173`; aceite escreve `FD`, `app/ponto/services/declarar_dia.py:190`.
3. Plano só pede relavra em `app/escala/folgas.py:395`; aceite também fecha perguntas vivas e tenta reconciliar chamados: `app/ponto/services/declarar_dia.py:204` → `app/ponto/services/veredito_celula.py:174`, `:177`, `:182`, `:183`.
4. Aceite carimba pergunta, fecha sua via administrativa e registra `val_perg_folga`: `app/chamados/services/validacao.py:290`, `:299`, `:300`; Plano não passa por esse escritor, conforme cadeia `app/escala/views_plano_folgas.py:185` → `app/escala/folgas.py:362`.

**Leitor e gravidade:** `app/ponto/precedencia.py:356` lê a folga e `:384` a transforma em fato `folga`. **Muda dia: sim, potencialmente**, sobretudo para dia anterior à competência atual que o Plano recusa e o aceite pode gravar; ocorrência e dinheiro efetivo em produção [não verificado]. Ambas as rotas relavram por `app/escala/folgas.py:272` e preservam a guarda de competência entregue em `app/ponto/portas/celula.py:214`.

**Convergência dos aceites com validação divergente:** detalhe só admite disputa aberta (`app/chamados/views.py:495`); inline também procura aberta (`app/chamados/views_cobrar.py:160`). Modal admite pergunta viva em disputa fechada (`app/chamados/services/fio.py:596`, `:601`, `:605`) e acrescenta trilha em `:611`. Portanto detalhe × modal e inline × modal são **mais dois pares divergentes**, embora compartilhem escritor; detalhe × inline converge no mesmo escritor, sem divergência adicional comprovada para folga.

**Fronteira varrida:** quatro views aptas ao aceite — detalhe, fio, modal e inline, nas referências acima e `app/chamados/views.py:659`, `:674`. O template do fio completo só tem formulário de mensagem/status (`app/templates/chamados/fio_colaborador.html:257`); disparador visual `validar_pergunta` nessa tela [não verificado], então não o contei nos três pares Plano × telas.

`validar_lote` e `validar_lote_tela` **não escrevem folga**: `FOLGA_CONTESTA` vira classe C por substring `folga`, `app/chamados/services/validacao.py:755`, `:806`, `:808`; lote recusa qualquer classe diferente de A em `:688`. Toggle tem view escritora em `app/escala/views_plano_folgas.py:98`, mas disparador em template [não verificado].

## 06/11 21:24 · sessao f30767be6376 · - · - · origem=Ronald (tmux)

Fechamento cruzado: **há duas representações de férias que as telas podem alterar separadamente**; não encontrei sincronização inversa `Ausencia → AgendamentoFerias` nos escritores/modelo/signals examinados. Evidências: `app/ponto/models.py:868` até o `super().save()` em :908; receivers de Ausencia em `app/ponto/signals.py:143`, :150, :275 e :282; consumidor limita-se a perguntas/chamados em signals:250. Sincronização existente tem sentido `AgendamentoFerias → Ausencia`, `app/ferias/models.py:434` → `_sync_ausencia`, models:379 → `app/ponto/services/ausencia.py:1160`.

| Ação cruzada | Escritores/efeitos diferentes | Leitor e gravidade |
|---|---|---|
| Conceder férias via `/ponto/ausencias/lancar/` × agendar férias | `lancar_ausencia`, `app/ponto/views.py:2368`, chama `criar_ausencia` com tipo recebido e status aprovada em :2415. Produz apenas Ausencia; montagem dos campos em `app/ponto/services/ausencia.py:341`. Já agendamento escreve Agendamento e `periodo.dias_concedidos`, `app/ferias/services.py:321`, e espelha Ausencia por model save, models:434. | Espelho vê cobertura aprovada, `app/ponto/services/espelho.py:588`; saldo depende de concedidos/vendidos, `app/ferias/models.py:216`; folha procura **Agendamento** em `app/folha/export.py:491`, retenção :868. **Sim, muda dia/saldo/retenção potencialmente**: conceder só Ausencia abona cobertura sem consumir saldo nem compor o mesmo universo de férias com batida. Ocorrência real **[não verificado]**. |
| Justificar dia com tipo férias × agendar férias | `/ponto/celula/veredito/`, `app/ponto/urls.py:58`, view `veredito_celula_view`, `app/ponto/views.py:3312`, passa tipo da tela em :3341. Escritor `veredito_celula`, `app/ponto/services/veredito_celula.py:191`, cria apenas Ausencia aprovada em :315 e ainda fecha fio em :325, lavra célula :330 e registra log :334. Não cria Agendamento/saldo; contraste com services férias:321. | Mesmos leitores/impacto acima; efeito adicional no fio/célula só nesse caminho, veredito_celula:324. |
| Editar Ausencia de férias × editar datas do Agendamento | `editar_ausencia`, chamada em `app/ponto/views.py:2794` → `editar_ausencia_registro`, `app/ponto/services/ausencia.py:465`, altera só tipo/range/minutos da Ausencia em :498, salva :509 e invalida fechamento :524. Editar Agendamento usa `_trocar_janela`, `app/ferias/services.py:366`, muda também Agendamento/concedidos e remove Ausencias antigas em :378. | **Sim, muda dia**: a cobertura pode sair da janela original sem mover a janela do Agendamento, preservando saldo e retenção calculada sobre o intervalo antigo; leitores espelho:588 e export:491. Existência desse estado em produção **[não verificado]**. |
| Cancelar Ausencia de férias × cancelar Agendamento | Cancelamento lote não exclui tipo férias: busca qualquer Ausencia não rejeitada, `app/ponto/views.py:2345`, decide rejeitada em :2351. `decidir`, `app/ponto/services/ausencia.py:897`, muda estado e salva campos da Ausencia, :911/:927. Cancelar Agendamento atualiza concedidos/status/obs, `app/ferias/services.py:352`, e save remove espelhos, `app/ponto/services/ausencia.py:1188`. | **Sim, muda dia**: cancelamento somente da Ausencia retira cobertura do espelho, mas Agendamento continua no universo `ferias_na_janela` enquanto não cancelado, `app/ferias/services.py:51`; folha ainda pode reter por férias com batida, export:868; saldo não recebe devolução. Ocorrência real **[não verificado]**. |

**Condição de disponibilidade do tipo:** catálogo congelado inclui `ferias`, `app/ponto/catalogo/ausencias.py:101`; `vigente()` usa banco ou catálogo congelado, :278; choices agrupadas incluem todos os códigos, :313; painel do colaborador exclui só `falta`, `app/colaboradores/views.py:1959`. Portanto os caminhos aceitam férias se o catálogo vigente a contiver; disponibilidade efetiva de férias no cadastro de produção **[não verificado]**. `Ausencia.exige_documento` declara exceção de férias no fluxo próprio, `app/ponto/models.py:727`, reforçada por `FORA_DO_A4 = ('ferias',)`, `app/ponto/services/ausencia.py:751`; não é veto de criação pelo admin.

**Venda histórica:** `criar_periodo_aquisitivo` lê `dias_vendidos`, `app/ferias/views.py:194`, e passa para `registrar_gozo_historico`, :210; escritor incrementa vendidos junto do gozo, `app/ferias/services.py:300`, enquanto venda separada usa `vender_dias_do_periodo`, services:507. Porém os templates examinados não oferecem campo de vendidos no formulário histórico: `app/templates/ferias/_secao_colab.html:6`–:33 e `criar_periodo.html:9`–:30. Assim o segundo **escritor** está comprovado, mas o segundo **disparo de tela de venda** continua **[não verificado]**; não contar como par de ação visual fechado sem achar formulário adicional.

_2588 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
