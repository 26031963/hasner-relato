# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 13:32** · publicado em **31/10/2026 13:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 13:32 · sessao 693245cde62f · - · - · origem=Ronald (tmux)

- **Pouso v3 sem prova suficiente nos anexos:** a própria declaração exige ensaio `--apply` anterior ao pouso com dias rejulgados, dias que movem minuto e `FORA-DO-HORIZONTE 0`; esses três números não foram fornecidos. Cumprimento do pré-requisito: **[não verificado]**. Não confundir ausência de prova com prova de pouso indevido — `app/ponto/services/cartorio.py:173-175`. A reconstrução das projeções também integra o pouso — `app/docs/LEIS.md:142`.

- **Horizonte e relógio:** `versao_para` usa declaração literal e o juiz existente `janela_atual`; não introduz leitura de relógio ou decisão pela exportação — `app/ponto/services/cartorio.py:188-207`. O teste atualizado verifica o leitor vivo, conservando 09 na versão 1, elevando 10 à versão 3 e deixando 08 fora — `app/ponto/tests/test_o219_versao_da_regra.py:78-93`.

- **Segundo juiz:** não identifiquei nova cópia do predicado no trecho alterado: `fatos_do_dia`, registrado em `app/core/juizes.py:160-161`, delega ao período em `app/ponto/precedencia.py:255-257`; período e grade usam o mesmo seletor — `app/ponto/precedencia.py:358`, `app/escala/utils.py:1115`, `app/escala/utils.py:1124`. O predicado ficou num único local — `app/ponto/precedencia.py:236-240`.

- **Segundo escritor, relógio e ordem de trava:** o carregador faz leitura ORM e o overlay altera o resultado em memória; os trechos novos não adicionam escrita persistente, aquisição de trava ou relógio — `app/ponto/precedencia.py:205-210`, `app/escala/utils.py:1114-1128`.

- **Força dos testes:** os testes da grade verificam tanto a grade quanto a precedência para os cinco cenários e os dois builders; a docstring reconhece controles que já passavam — `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:21-25`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:70-104`. O controle do calendário só exige `status != 'folga'`, portanto também aceita um status incorreto diferente de folga; não prova o `falta` prometido na docstring — `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:10`, `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:58-63`. Execução RED/verde: **[não verificado]**.

## 31/10 13:32 · sessao 5e1b6dd36cd1 · - · - · origem=Ronald (tmux)

Achado de incompletude: `eh_trabalho` ainda sai de caminho paralelo. O calendário calcula pelo vínculo ativo e `_folgas_mes` (`app/colaboradores/services/calendario.py:772`), enquanto `status` lê `faixa` e `fatos['folga_lancada']` (`app/colaboradores/services/calendario.py:795`, `app/colaboradores/services/calendario.py:803`). `_classificar` troca somente sua variável local `trabalho` para `False` (`app/colaboradores/services/calendario.py:111`, `app/colaboradores/services/calendario.py:116`); o dicionário devolve o `eh_trabalho` original (`app/colaboradores/services/calendario.py:890`).

No próprio caso novo, a célula declara `trabalha=True` (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:40`), e `eh_dia_trabalho` retorna esse valor antes de consultar folgas (`app/escala/models.py:1071`). Portanto, por leitura estática, a folga manual do velho produz `status='folga'` e `eh_trabalho=True`; o teste cobra exclusivamente o primeiro campo (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:55`). Execução vermelho/verde: [nao verificado].

A autoridade de `fatos['folga_lancada']` é `fatos_do_periodo`, pelo seletor comum (`app/ponto/precedencia.py:358`) e escrita booleana (`app/ponto/precedencia.py:408`); `faixa` vem da ordenação da precedência (`app/ponto/precedencia.py:392`, `app/ponto/precedencia.py:400`). O calendário passa a alcançar essa autoridade sem fornecer `folgas_lancadas` (`app/colaboradores/services/calendario.py:604`). Os contadores seguem os `status` já classificados (`app/colaboradores/services/calendario.py:937`, `app/colaboradores/services/calendario.py:944`); badges continuam lendo feriados e rubricas lavradas (`app/colaboradores/services/calendario.py:838`, `app/colaboradores/services/calendario.py:840`, `app/colaboradores/services/calendario.py:845`).

Restam `_folgas_mes` como alimentação de `folgas_foto` (`app/colaboradores/services/calendario.py:606`) e do cálculo paralelo de trabalho (`app/colaboradores/services/calendario.py:773`). O fallback por foto trata uma data presente como folga sem examinar sua origem (`app/escala/models.py:1252`, `app/escala/models.py:1280`, `app/escala/models.py:1288`).

O controle do calendário velho não demonstra a regressão corrigida: lança folga de calendário no vínculo velho, mas a foto antiga do calendário era a do ativo (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:60`, `app/colaboradores/services/calendario.py:593`). Além disso, apenas cobra `status != 'folga'`, aceitando inclusive estados diferentes do `falta` prometido na docstring (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:10`, `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:62`). Ele serve como controle negativo fraco; não cobre o caso citado no comentário da cura — calendário do ativo num dia cuja geradora é outra (`app/colaboradores/services/calendario.py:608`).

## 31/10 13:30 · sessao ebfc1284978e · F1C4-3-EMISSORES · verificador · origem=Code

O corte de texto pode destruir a chave de deduplicação da pauta; os testes atuais não exercitam esse limite (`app/pautas/services.py:171–177`; `app/chamados/tests/test_f1c4_3_emissores.py:218–257`). O inventário **completo** de `4236474c..49b05829` fica **[não verificado]**: a cópia não contém histórico Git nem esse comparativo; o anexo fornecido cobre `41b4d89d..49b05829` **[não verificado quanto à base pedida]**.

1. As mutações abaixo são conclusões por leitura estática; o resultado de sua execução está **[não verificado]**.

| Teste de histórico | Mutação mínima que o deixaria vermelho | O que distingue | [nao verificado]
|---|---|---|
| `test_RED_limbo_em_exportada_duas_vezes_uma_pauta` | Em `fabrica_da_celula.py:183`, trocar a seleção de motivos novos por `novos = motivos`: o segundo julgamento criaria outra pauta, contrariando a contagem final (`app/chamados/services/fabrica_da_celula.py:183–190`; `app/chamados/tests/test_f1c4_3_emissores.py:218–225`). | **Não detecta** restringir o histórico a `objects.vivas()`: a primeira pauta continua viva (`app/pautas/models.py:57–64`; `app/chamados/tests/test_f1c4_3_emissores.py:218–225`). |
| `test_RED_limbo_em_exportada_pauta_feita_nao_reabre` | Acrescentar `feito_em__isnull=True` à consulta histórica: a feita seria ignorada e outra pauta nasceria (`app/chamados/services/fabrica_da_celula.py:180–190`; `app/chamados/tests/test_f1c4_3_emissores.py:233–240`). | Detecta `objects.vivas()`, mas não a exclusão **somente das canceladas** (`app/pautas/models.py:57–64`; `app/chamados/tests/test_f1c4_3_emissores.py:227–241`). |
| `test_RED_limbo_em_exportada_pauta_cancelada_nao_reabre` | Acrescentar `cancelado_em__isnull=True`: a cancelada seria ignorada e outra pauta nasceria (`app/chamados/services/fabrica_da_celula.py:180–190`; `app/chamados/tests/test_f1c4_3_emissores.py:249–256`). | Detecta `objects.vivas()`, mas não a exclusão **somente das feitas** (`app/pautas/models.py:57–64`; `app/chamados/tests/test_f1c4_3_emissores.py:243–257`). |

Quanto às portas:

- **Feita:** substituir `marcar_feita_pelo_sistema` por um no-op, ou escrever `feito_em` apenas em memória, é detectado: o teste recarrega e exige o campo persistido. A viva e a cancelada não chamam essa porta (`app/chamados/tests/test_f1c4_3_emissores.py:218–251`; `app/pautas/services.py:264–266`).
- **Feita:** remover a escrita de `lido_em`, o recarregamento interno, `_trilha` ou `lavrar` passa por esses testes, desde que `feito_em` seja persistido; nenhuma dessas outras obrigações é afirmada (`app/pautas/services.py:264–268`; `app/chamados/tests/test_f1c4_3_emissores.py:233–241`).
- **Cancelada:** no-op, escrita apenas em memória ou escrita de outro timestamp são detectados pelo recarregamento e pela exigência de `cancelado_em` preenchido (`app/pautas/services.py:286–287`; `app/chamados/tests/test_f1c4_3_emissores.py:249–251`).
- **Cancelada:** remover autorização do autor, recusa de pauta feita, guarda contra cancelamento repetido, recarregamento interno, trilha ou contador passa nesses casos: a chamada acontece uma vez, pelo autor, numa pauta ainda não feita (`app/pautas/services.py:280–289`; `app/chamados/tests/test_f1c4_3_emissores.py:247–257`).

Há duas lacunas diretamente relacionadas ao histórico: retirar `pai__isnull=True` passa porque nenhum caso cria uma resposta; e limpar `feito_em`/`cancelado_em` no segundo julgamento também pode passar, pois os timestamps são conferidos **antes**, sem nova conferência do estado final (`app/chamados/services/fabrica_da_celula.py:180`; `app/chamados/tests/test_f1c4_3_emissores.py:218–257`).

2. Para o corte, sejam `C = len(_ctx)`, `T = len(texto_original)` e `e` a posição imediatamente após o marcador ou uma tag. O espaço calculado é **`496 − C`**, e ainda há um `[:500]` no momento de gravar (`app/pautas/services.py:169–177`).

Para `C ≤ 496`, o elemento inteiro sobrevive quando `496 − C ≥ e`. O marcador tem **30 caracteres**; portanto fica inteiro até **C=466** e é mutilado entre **467 e 496**. As tags vêm depois dele, então começam a desaparecer antes (`app/chamados/services/fabrica_da_celula.py:164`; `app/chamados/services/fabrica_da_celula.py:189–190`; `app/pautas/services.py:171`).

| Conteúdo original | Último `C ≤ 496` que preserva marcador e todas as tags |
|---|---:|
| Uma tag `[motivo:orfao_14h]` | **447** |
| Uma tag `[motivo:saida_sem_entrada]` | **439** |
| Uma tag `[motivo:intervalo_saida]` ou `[motivo:intervalo_volta]` | **441** |
| As quatro tags acima juntas | **370** |

Os limites da tabela resultam dos nomes definidos em `app/chamados/services/declaracao_texto.py:42–46`, do formato em `app/chamados/services/fabrica_da_celula.py:189–190` e do corte em `app/pautas/services.py:171`.

**Acima de 496, o índice fica negativo.** `texto[:-1]` conserva tudo menos o último caractere; não significa “nenhum espaço disponível”. Para `C>496`, o tamanho conservado do original é `max(0, T + 496 − C)`, antes da concatenação e do corte final (`app/pautas/services.py:171–177`).

Isso torna a perda **não monotônica**. Para o texto com somente `[motivo:orfao_14h]`, cujo tamanho original calculado é 154, acontece o seguinte (`app/chamados/services/fabrica_da_celula.py:164`; `app/chamados/services/fabrica_da_celula.py:189–190`; `app/pautas/services.py:171–177`):

| Comprimento do contexto | Marcador inteiro | Tag inteira |
|---|---|---|
| Até 447 | Sim | Sim |
| 448–466 | Sim | Não |
| 467–496 | Não | Não |
| 497–601 | Sim, reaparece | Sim, reaparece |
| 602–620 | Sim | Não |
| A partir de 621 | Não | Não |

Com **C=496**, o original vira `texto[:0]`; com **C=497**, vira `texto[:-1]`; nesse exemplo, somente a partir de **C=650** o índice negativo elimina o original inteiro (`app/pautas/services.py:171`; formato original em `app/chamados/services/fabrica_da_celula.py:189–190`).

O comprimento do contexto depende destes componentes:

| Componente | Limitação verificável |
|---|---|
| Nome | `CharField(max_length=200)`; incluído sem corte adicional (`app/colaboradores/models.py:320`; `app/pautas/services.py:419–420`). |
| Matrícula | `IntegerField`, convertido em texto, ou `'-'` (`app/colaboradores/models.py:302–305`; `app/pautas/services.py:420`). |
| Marcos | Até seis chaves canônicas; seus valores JSON são concatenados sem limite individual nessa função (`app/escala/models.py:1620–1647`; `app/pautas/services.py:406–410`). |
| Batidas | Todas as apuráveis da janela de 39 horas, sem teto de quantidade; hora formatada em cinco caracteres mais o tipo de um caractere (`app/pautas/services.py:411–413`; `app/ponto/models.py:24`; `app/ponto/turnos.py:2845–2852`). |
| Acusação | Veredito limitado a 20 caracteres, acrescido de todas as horas apagadas da ata, sem corte de quantidade ou tamanho nessa composição (`app/escala/models.py:1738`; `app/pautas/services.py:414–417`). |
| Data e link | Data formatada, rota com identificação do colaborador e parâmetros de mês/ano (`app/pautas/services.py:418–420`; `app/colaboradores/urls.py:24`). |

Logo, **não existe teto de 496 para `_ctx`**. Perder o marcador faz a consulta histórica ignorar a pauta inteira; perder uma tag faz aquele motivo não entrar em `ja`, permitindo escrevê-lo novamente (`app/pautas/services.py:392–420`; `app/chamados/services/fabrica_da_celula.py:180–190`). Incidência e comprimentos reais em produção: **[não verificado]**.

3. Não é possível certificar a lista completa de campos alterados desde `4236474c` **[não verificado]**. Este é o mapa verificável dos campos cujo uso muda no delta fornecido:

| Campo | Quem escreve | Quem lê no caminho alterado |
|---|---|---|
| `CelulaDia.trabalha` | Geração, regeneração e restauração pela porta (`app/ponto/portas/celula.py:590–615`; `:243–265`; `:532–538`). | `dia_sem_trabalho`, chamado pelos quatro emissores; o desvio para DP ainda testa `None` diretamente (`app/chamados/juizes.py:407–408`; `app/chamados/services/fabrica_da_celula.py:80`; `:153`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`). |
| `ata.tipo_dia` | Cartório monta; porta atribui a ata (`app/ponto/services/cartorio.py:567`; `app/ponto/portas/celula.py:767–768`). | Juiz único (`app/chamados/juizes.py:407–408`). |
| `ata.sem_ancora` | Grade produz e cartório copia (`app/escala/utils.py:729`; `:1514`; `:1701–1702`; `app/ponto/services/cartorio.py:570`). | Cluster e intervalo deixam de usá-lo como veto; passam ao juiz, que não consulta essa chave (`app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; `app/chamados/juizes.py:407–408`). |
| `Pauta.ancora_tipo`, `ancora_id`, `destinatario_departamento` | Emissor fornece `dia`, `colaborador:data`, `dp`; `escrever` persiste (`app/chamados/services/fabrica_da_celula.py:175–186`; `app/pautas/services.py:172–177`). | Deduplicação histórica; listagem, chip e contador (`app/chamados/services/fabrica_da_celula.py:180–181`; `app/pautas/services.py:314–315`; `:328–342`). |
| `Pauta.texto`, marcador e tags | Emissor fabrica; porta acrescenta contexto e corta (`app/chamados/services/fabrica_da_celula.py:189–190`; `app/pautas/services.py:163–177`). | `startswith` reconhece marcador; substring reconhece cada motivo (`app/chamados/services/fabrica_da_celula.py:180–182`). |
| `Pauta.pai` | Emissor omite argumento, usando `None`; respostas fornecem o pai (`app/pautas/services.py:155`; `:177`; `:192`). | Guarda histórica exige cabeça (`app/chamados/services/fabrica_da_celula.py:180`). |
| `remetente_departamento`, `autor_user` | Autor de sistema e remetente `ti` fornecidos pelo emissor; resolução e persistência na porta (`app/chamados/services/fabrica_da_celula.py:185–186`; `app/pautas/services.py:157–177`). | Resposta consulta remetente/autor; cancelamento consulta autor (`app/pautas/services.py:188–192`; `:280–281`). |
| `feito_em`, `lido_em`, `cancelado_em` | Portas de leitura, fechamento humano, fechamento de sistema e cancelamento (`app/pautas/services.py:207`; `:239–241`; `:264–266`; `:286–287`). | `vivas()` usa os estados; a guarda nova deliberadamente consulta todo o histórico sem filtrá-los (`app/pautas/models.py:57–64`; `app/chamados/services/fabrica_da_celula.py:180–182`). |

Há também os campos E3 visíveis na cópia; sua inclusão no intervalo solicitado é **[não verificado]**:

- **`status_dia` e `status_rotulo`:** declarados no model e criados pela migration 0044; atribuídos por `_aplicar_status` e persistidos pela porta. O leitor público consulta ambos; a porta também os compara/conserva, e a fila D5 consulta `status_dia` (`app/escala/models.py:1784–1785`; `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:13–21`; `app/ponto/portas/celula.py:634–642`; `:697–706`; `app/ponto/status_dia.py:171–183`; `app/ponto/services/cartorio.py:726`).
- **`fatos_do_status`:** argumento transitório, com `tipo_dia`, `previsto_min`, `tem_batida`, `isento`, `cobertura_parcial` e `folga_humana`; produzido pelo helper, entregue pelo cartório, encaminhado pelo model e consumido pela regra da porta (`app/ponto/status_dia.py:124–149`; `app/ponto/services/cartorio.py:1563–1564`; `app/escala/models.py:1794–1796`; `app/ponto/portas/celula.py:693–698`).

Além do truncamento, encontrei estes defeitos ou limites:

- **Contradição com o aval literal (a):** o código permite pergunta nova em exportada quando `trabalha=True`, e o teste exige esse nascimento. Isso contraria “Pergunta NOVA […] só em competência ABERTA”; autorização posterior está **[não verificado]** (`app/chamados/services/fabrica_da_celula.py:153–160`; `app/chamados/tests/test_f1c4_3_emissores.py:212–216`).
- **Desvio para DP contornável:** o ramo de fio mudo seleciona chamado vivo sem filtro de `trabalha`, agrega os motivos e chama `perguntar` diretamente. Um limbo exportado com chamado vivo e motivo ainda sem pergunta pode alcançar esse caminho sem consultar a pauta; o lote está configurado com `--apply` (`app/chamados/management/commands/disparar_perguntas_competencia.py:155–168`; `:220–244`; `:276–289`; `app/chamados/services/fabrica_da_celula.py:114–126`; `app/config/crons.py:1464–1466`). Ocorrência real: **[não verificado]**.
- **Deduplicação vulnerável à concorrência:** consulta e criação não estão protegidas conjuntamente; duas execuções podem ler ausência antes de inserir. A transação começa dentro de `escrever`, e não existe unicidade por dia/motivo no modelo; serialização externa e ocorrência real estão **[não verificado]** (`app/chamados/services/fabrica_da_celula.py:180–190`; `app/pautas/services.py:152–177`; `app/pautas/models.py:123–132`).
- **COL717 não está provado:** os testes usam colaborador sintético, sem reproduzir seu chamado existente; preservação do único chamado e ausência de porta nova para COL717 estão **[não verificado]** (`app/chamados/tests/test_f1c4_3_emissores.py:57–65`). Também não se deve equiparar achado de intervalo a chamado novo: essa emissão está aposentada; cluster, por sua vez, chama a porta de abertura (`app/ponto/management/commands/detectar_intervalo_ausente.py:149–157`; `app/ponto/services/cluster_espurio.py:87–105`).

## 31/10 13:28 · sessao 2e02f2c7fa32 · - · - · origem=Ronald (tmux)

O intervalo completo `4236474c..49b05829` não é reconstruível nesta cópia sem histórico Git nem diff dessa base; atribuir cada campo a esse intervalo fica **[não verificado]**. O `emis5_diff.txt` fornecido cobre outra base (`41b4d89d`); nele há novas leituras/escritas de campos existentes, sem alteração de models/migrations **[não verificado quanto ao intervalo pedido]**.

Mapa verificável dos campos alcançados pelo emissor:

| Campo | Quem escreve na cópia | Quem lê no caminho alterado |
|---|---|---|
| `CelulaDia.trabalha` | Geração em `app/ponto/portas/celula.py:590-615`; regeneração em `app/ponto/portas/celula.py:243-265`; restauração em `app/ponto/portas/celula.py:532-538`. | Juiz `app/chamados/juizes.py:407-408`, chamado pelos quatro emissores em `app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/materializacao.py:160`, `app/ponto/services/cluster_espurio.py:28`, `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; o ramo novo também distingue `None` diretamente em `app/chamados/services/fabrica_da_celula.py:153`. |
| `ata.tipo_dia` | Construído em `app/ponto/services/cartorio.py:567`, entregue à porta em `app/ponto/services/cartorio.py:1557-1559`, atribuído em `app/ponto/portas/celula.py:767-768`. | Juiz em `app/chamados/juizes.py:407-408`. |
| `ata.sem_ancora` | Origem na grade em `app/escala/utils.py:729`, `app/escala/utils.py:1514`, `app/escala/utils.py:1701-1702`; copiado para ata em `app/ponto/services/cartorio.py:570`. | Os novos gates de cluster/intervalo leem o juiz em `app/ponto/services/cluster_espurio.py:28` e `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; esse juiz não consulta `sem_ancora` em `app/chamados/juizes.py:407-408`. |
| `Pauta.ancora_tipo`, `ancora_id`, `destinatario_departamento` | Novo chamador fornece `dia`, `colab:data`, `dp` em `app/chamados/services/fabrica_da_celula.py:175-186`; porta persiste em `app/pautas/services.py:172-177`. | Guarda histórica em `app/chamados/services/fabrica_da_celula.py:180-181`; chip em `app/pautas/services.py:328-329`; listagem e contador em `app/pautas/services.py:314-315`, `app/pautas/services.py:340-342`. |
| `Pauta.texto`: marcador e tags `[motivo:…]` | Fabricados em `app/chamados/services/fabrica_da_celula.py:189-190`; contexto e truncamento em `app/pautas/services.py:163-177`. | Marcador pelo `startswith`, tags por substring em `app/chamados/services/fabrica_da_celula.py:180-182`. |
| `Pauta.pai` | Novo emissor omite argumento, portanto usa `None` de `app/pautas/services.py:155`, persistido em `app/pautas/services.py:177`; respostas passam pai em `app/pautas/services.py:192`. | Dedup exige cabeça em `app/chamados/services/fabrica_da_celula.py:180`. |
| `Pauta.remetente_departamento`, `autor_user` | Novo emissor fornece autor e remetente informado em `app/chamados/services/fabrica_da_celula.py:185-186`; resolução em `app/pautas/services.py:157`; gravação em `app/pautas/services.py:173-177`. | Autoria autoriza cancelamento em `app/pautas/services.py:280-281`; remetente e autor orientam resposta em `app/pautas/services.py:188-192`. |
| `feito_em`, `lido_em`, `cancelado_em` | Fecho da máquina em `app/pautas/services.py:264-266`; cancelamento em `app/pautas/services.py:286-287`; marcação humana em `app/pautas/services.py:239-241`. | Definição de viva em `app/pautas/models.py:57-64`; a nova guarda histórica não os filtra em `app/chamados/services/fabrica_da_celula.py:180-182`. |

Apêndice E3 — candidatos visíveis na cópia, cuja inclusão no intervalo `4236474c..49b05829` é **[não verificado]**:

- **`status_dia` e `status_rotulo`:** campos em `app/escala/models.py:1784-1785`; criação explícita por `AddField` em `app/escala/migrations/0044_celuladia_status_dia_celuladia_status_rotulo.py:13-21`. A porta atribui ambos em `app/ponto/portas/celula.py:701-706` e persiste por `salvar_e_emitir` em `app/ponto/portas/celula.py:634-642`; entradas pela lavra em `app/ponto/portas/celula.py:739`, `app/ponto/portas/celula.py:752-775`.
- **Leitores das colunas E3:** leitor público em `app/ponto/status_dia.py:171-183`; comparação/conservação interna em `app/ponto/portas/celula.py:697`, `app/ponto/portas/celula.py:703`; fila D5 lê `status_dia` em `app/ponto/services/cartorio.py:726`. O próprio módulo declara que as telas ainda aguardam E4 em `app/ponto/status_dia.py:8-9`.
- **`fatos_do_status`:** é argumento/dicionário transitório, não coluna; produzido em `app/ponto/status_dia.py:124-149`, entregue pelo cartório em `app/ponto/services/cartorio.py:734-735`, `app/ponto/services/cartorio.py:1046-1047`, `app/ponto/services/cartorio.py:1563-1564`, encaminhado pelo model em `app/escala/models.py:1794-1796`, consumido pela porta/regra em `app/ponto/portas/celula.py:693-698`. Suas chaves são `tipo_dia`, `previsto_min`, `tem_batida`, `isento`, `cobertura_parcial`, `folga_humana`, em `app/ponto/status_dia.py:146-149`; leitura pela regra em `app/ponto/status_dia.py:70-121`.
- **Homônimos de apresentação:** `status_rotulo` em `app/escala/views.py:55` e `app/colaboradores/services/calendario.py:884` é chave de payload derivada, não leitura da coluna `CelulaDia.status_rotulo`; a coluna é lida explicitamente pelo leitor em `app/ponto/status_dia.py:182`.

Outro defeito verificável: a deduplicação nova não é atômica entre consulta e criação: lê histórico em `app/chamados/services/fabrica_da_celula.py:180-183` e depois chama a escrita em `app/chamados/services/fabrica_da_celula.py:185`; a transação começa somente dentro de `escrever`, em `app/pautas/services.py:152-153`, e as constraints de `Pauta` não impõem unicidade desse fato em `app/pautas/models.py:112-132`. Assim, duas execuções concorrentes podem ler ausência e criar duas pautas; ocorrência em produção **[não verificado]**.

Outro caminho que merece constar: o ramo de fio mudo do lote não filtra `trabalha` em `app/chamados/management/commands/disparar_perguntas_competencia.py:155-168`, agrega seus alvos em `app/chamados/management/commands/disparar_perguntas_competencia.py:244` e chama `perguntar` diretamente em `app/chamados/management/commands/disparar_perguntas_competencia.py:276-289`, sem passar pelo novo encaminhamento para Pauta de `app/chamados/services/fabrica_da_celula.py:153-157`. Já o caminho principal do lote exclui limbo pelo filtro `trabalha=True` em `app/chamados/management/commands/disparar_perguntas_competencia.py:76`; portanto ele também não mede a nova elegibilidade do evento. Incidência real nesses universos **[não verificado]**.

## 31/10 13:28 · sessao 04f7ce2e5937 · - · - · origem=Ronald (tmux)

Achados adicionais, por leitura estática:

- O aval fornecido diz que pergunta **nova só nasce em competência aberta**, mas a implementação restringe o desvio para DP a `trabalha is None`; com `True`, chama `perguntar` mesmo em exportada. O teste exige essa pergunta nascer: `app/chamados/services/fabrica_da_celula.py:153-160`; `app/chamados/tests/test_f1c4_3_emissores.py:212-216`. O cumprimento desse aval está contradito por código e teste; eventual autorização posterior [não verificado].

- O novo bloqueio pode ser contornado pelo caminho **fio mudo do lote**: seleciona chamados vivos com célula, sem filtro de `trabalha`, calcula motivos, e entrega diretamente a `perguntar`, que não consulta competência nem histórico de Pauta. Logo, limbo exportado com chamado vivo e fato ainda sem pergunta pode voltar ao app por esse caminho: `app/chamados/management/commands/disparar_perguntas_competencia.py:155-168`, `:181-226`, `:276-289`; `app/chamados/services/fabrica_da_celula.py:114-126`. O lote está configurado com `--apply`: `app/config/crons.py:1464-1466`. A primeira via do lote filtra `trabalha=True`, portanto não atribuir a ela o limbo: `app/chamados/management/commands/disparar_perguntas_competencia.py:76`.

- A idempotência nova não é protegida contra concorrência dentro da função: consulta o histórico, decide `novos` e depois cria; duas execuções podem ler histórico vazio antes das duas inserções. A transação de `escrever` começa somente na criação, e o modelo não declara unicidade por dia/motivo: `app/chamados/services/fabrica_da_celula.py:180-190`; `app/pautas/services.py:152-177`; `app/pautas/models.py:123-132`. Ocorrência efetiva em produção e eventual serialização externa [não verificado].

- Não confundir resultado de `furos_intervalo` com abertura de chamado: o comando mantém apenas reconciliação de legado e sua emissão está aposentada por `continue`: `app/ponto/management/commands/detectar_intervalo_ausente.py:122-139`, `:149-157`. Já cluster pode abrir um chamado por dia encontrado: `app/ponto/services/cluster_espurio.py:23-35`, `:87-105`.

- O efeito específico sobre COL717 não está demonstrado pelo dado fornecido [não verificado]. Não se pode concluir que uma nova porta efetivamente nasceu ou que o único chamado foi preservado apenas pelos testes apresentados [não verificado]. [nao verificado]

## 31/10 13:28 · sessao 59f02be287a2 · - · - · origem=Ronald (tmux)

Análise estática; execução dos mutantes [não verificado].

| Teste de histórico | Mutação mínima da produção que o deixaria vermelho | Limite da prova | [nao verificado]
|---|---|---|
| `test_RED_limbo_em_exportada_duas_vezes_uma_pauta` | Trocar `novos = [m for m in motivos if m not in ja]` por `novos = motivos`: o segundo julgamento escreveria outra pauta e `len(...) == 1` falharia. (`app/chamados/services/fabrica_da_celula.py:183-190`; `app/chamados/tests/test_f1c4_3_emissores.py:218-225`) | Trocar a consulta histórica por `Pauta.objects.vivas().filter(...)` **não** é detectado por este teste: a primeira pauta permanece viva; `vivas()` admite pautas sem `feito_em` e sem `cancelado_em`. (`app/chamados/tests/test_f1c4_3_emissores.py:218-225`; `app/pautas/models.py:57-64`) |
| `test_RED_limbo_em_exportada_pauta_feita_nao_reabre` | Acrescentar `feito_em__isnull=True` à consulta da deduplicação: a pauta feita deixaria de fornecer os motivos já vistos; surgiria outra e a contagem falharia. (`app/chamados/services/fabrica_da_celula.py:180-190`; `app/chamados/tests/test_f1c4_3_emissores.py:233-240`) | Também detectaria a troca por `objects.vivas()`, mas não prova preservação de `feito_em` após o segundo julgamento: o timestamp é conferido **antes**, e depois só se conferem retorno, quantidade e perguntas. (`app/pautas/models.py:57-64`; `app/chamados/tests/test_f1c4_3_emissores.py:234-241`) |
| `test_RED_limbo_em_exportada_pauta_cancelada_nao_reabre` | Acrescentar `cancelado_em__isnull=True` à consulta da deduplicação: a cancelada seria ignorada, outra pauta nasceria e a contagem falharia. (`app/chamados/services/fabrica_da_celula.py:180-190`; `app/chamados/tests/test_f1c4_3_emissores.py:249-256`) | Também detectaria `objects.vivas()`, mas não prova preservação de `cancelado_em` após o segundo julgamento: a conferência do timestamp antecede esse julgamento. (`app/pautas/models.py:57-64`; `app/chamados/tests/test_f1c4_3_emissores.py:250-257`) |

Quanto às mutações das portas:

- **`marcar_feita_pelo_sistema`:** o teste da feita detecta um `return pauta` sem escrita, ou escrita somente no objeto em memória: ele recarrega do banco e exige `feito_em` preenchido. O teste da viva e o da cancelada não chamam essa porta. (`app/chamados/tests/test_f1c4_3_emissores.py:218-235`, `:243-251`; `app/pautas/services.py:248-266`)
- **`marcar_feita_pelo_sistema`:** remover a escrita de `lido_em`, o `refresh_from_db` interno, `_trilha` ou `lavrar` não é detectado pelas asserções desses testes, desde que `feito_em` continue persistido. O próprio teste faz seu recarregamento. (`app/pautas/services.py:264-268`; `app/chamados/tests/test_f1c4_3_emissores.py:233-241`)
- **`cancelar`:** o teste da cancelada detecta um no-op, escrita somente em memória ou troca de `cancelado_em` por outro campo: após recarregar, exige o timestamp correto. Os outros dois testes não chamam essa porta. (`app/chamados/tests/test_f1c4_3_emissores.py:218-241`, `:245-251`; `app/pautas/services.py:286-287`)
- **`cancelar`:** remover a autorização do autor, a recusa de pauta feita, a guarda de cancelamento repetido, o recarregamento interno, `_trilha` ou `lavrar` não é detectado por esses casos: há uma única chamada, pelo autor, em pauta ainda não feita, e as asserções não cobrem trilha ou contadores. (`app/pautas/services.py:280-289`; `app/chamados/tests/test_f1c4_3_emissores.py:247-257`)

Outras lacunas concretas desses testes: [nao verificado]

- Retirar `pai__isnull=True` da deduplicação passa pelos três casos: nenhum cria resposta com `pai`; portanto não há caso que diferencie cabeça de resposta. (`app/chamados/services/fabrica_da_celula.py:180`; `app/chamados/tests/test_f1c4_3_emissores.py:218-257`; `app/pautas/services.py:184-192`)
- Os testes não impedem uma regressão que **reabra a própria pauta**, limpando seu timestamp no segundo julgamento: exigem uma pauta ao final, mas não revalidam o estado final dela. (`app/chamados/tests/test_f1c4_3_emissores.py:234-241`, `:250-257`)
- O teste inicial de encaminhamento verifica apenas a presença do prefixo textual `'[motivo:'`; não exige o motivo completo e correto. Os casos históricos cobrem repetição do mesmo cenário, sem introduzir um segundo motivo posteriormente. (`app/chamados/tests/test_f1c4_3_emissores.py:199-205`, `:218-257`)

## 31/10 13:25 · sessao 2bdf6996f2ed · CLAUDE-MD-RACIONAL · verificador · origem=Code

A consolidação perde cláusulas explícitas, incluindo o registro do juiz de prazo e o prazo do DIFF da troca de motor. Portanto, não sustenta integralmente “fusão sem mudar regra”. (pai:CLAUDE.md:563–567, 661–662; novo:CLAUDE.md:594–595, 624–626)

Nas referências abaixo, **pai = `41b4d89d`** e **novo = `e25a0940`**.

1. **Cláusulas removidas sem reprodução nos consolidados**

| Trecho removido, com linha do pai | O que entrou no lugar |
|---|---|
| “sem fechamento, **MOSTRAR o numero do motor rotulado** — zero seria perda de informacao”. (pai:CLAUDE.md:520–521) | Mantém somente os dois critérios negativos do placar: nenhum número sem rótulo e nenhuma derivação própria. **Sem destino para a obrigação positiva de mostrar o número.** (novo:CLAUDE.md:567–570) |
| A pergunta antiga “quem **CHAMA** o motor”, mediante AST, “morre”. (pai:CLAUDE.md:522–523) | Conserva “EXERCICIO, nao CAMINHO”, mas **não reproduz a determinação sobre a pergunta antiga do selo**. (novo:CLAUDE.md:567–570) |
| Handoff deve conter HEAD/origin, status, ORDEM VIVA/item em curso **lidos do `hook_stop_fila1`**, PAREI, avais, processos vivos e próximo passo. (pai:CLAUDE.md:610–612) | Mantém comando, hooks e teto de 60 linhas. **Sem destino para a lista obrigatória e para o leitor canônico da fila.** (novo:CLAUDE.md:508–512) |
| Novo juiz de prazo: escrever a frase **e declarar em `JUIZES_DO_PRAZO`**. (pai:CLAUDE.md:661–662) | Mantém frase, citação da regra e número intocado. **Sem destino para a inscrição em `JUIZES_DO_PRAZO`.** (novo:CLAUDE.md:624–626) |
| “backup e renomeacao sao do admin”. (pai:CLAUDE.md:535) | Mantém nome canônico e armazenamento das versões. **Sem destino para essa atribuição de responsabilidade.** (novo:CLAUDE.md:589–593) |
| DIFF **ANTECIPADO** permanece em exportada **e troca de motor**, uma vez como impacto. (pai:CLAUDE.md:563–567) | Preserva “ANTES” na exportada; na troca de motor mantém uma vez/impacto, **sem destino para a antecipação**. (novo:CLAUDE.md:582–583, 594–595) |
| “Sem criterio escrito no aval, vale a L-009 como hoje.” (pai:CLAUDE.md:579) | Mantém condições do aval de critério e parada por violação, mas **não reproduz essa cláusula residual**; a L-009 aparece genericamente no cabeçalho. (novo:CLAUDE.md:575–576, 586–588) |

Outras cláusulas também desapareceram dos consolidados, **mas continuam expressas em fontes mantidas**:

| Trecho removido | Texto novo e preservação externa |
|---|---|
| Declaração de autoridade no CORTES deve vir “**citando este aval**”. (pai:CLAUDE.md:483) | A citação desaparece do item consolidado; continua em `PISTA.md`. (novo:CLAUDE.md:462–464; app/docs/PISTA.md:52–53) |
| Codex: “tarefa **so por delegacao do Claude**”. (pai:CLAUDE.md:504) | Exige wrapper, mas não reproduz quem delega; a restrição continua em `PISTA.md`. (novo:CLAUDE.md:521–528; app/docs/PISTA.md:72–74) |
| “o pendente **VOLTA se o contador sair do esperado**”. (pai:CLAUDE.md:653–654) | Diz apenas “frase de reversao”; gatilho e consequência continuam na L-118. (novo:CLAUDE.md:621; app/docs/LEIS.md:139) |
| Moratória identifica `contratos_estruturais`, seu juiz, nascimento por corte literal e extinção automática em 20/20. (pai:CLAUDE.md:644–647) | Resume para “Ate 20/20”; os detalhes continuam na L-117 referenciada. (novo:CLAUDE.md:455; app/docs/LEIS.md:138) |

Essa distinção importa porque a própria PISTA declara que seu texto literal prevalece sobre o resumo divergente. (app/docs/PISTA.md:8–12)

2. **Diferenças de condição, alcance ou prazo**

| Antes | Depois | Diferença |
|---|---|---|
| Bug novo que muda o desenho: parar e pôr no topo dos **PENDENTES**. (pai:CLAUDE.md:459–460) | Vai ao topo do **RELATO**, como pergunta de desenho com números; a obra para e as outras seguem. (novo:CLAUDE.md:458–459) | Troca o destino do registro e explicita uma parada limitada à obra. |
| Sonnet para leitura: **somente LIMITE 75 ou falha**, com motivo registrado. (pai:CLAUDE.md:506) | Agrupa “leitura/sonda” e admite também “**ou motivo na linha do DESPACHO**”. (novo:CLAUDE.md:532–534) | Alarga textualmente a exceção de leitura. A exceção por motivo já existia para **sonda**, enquanto leitura permanecia restrita a limite/falha. (app/docs/PISTA.md:92–93, 159–160) |
| Juiz de prazo existente deve responder à **MESMA pergunta**. (pai:CLAUDE.md:659–660) | Pergunta a juiz de `JUIZES_DO_PRAZO` “**se ja houver**”. (novo:CLAUDE.md:623–624) | Omite a condição explícita de correspondência da pergunta. |
| Não numerar nem propor **lei nova por iniciativa própria**. (pai:CLAUDE.md:645–646) | “nao numera nem propoe **lei**”. (novo:CLAUDE.md:455) | O enunciado fica mais restritivo: desaparecem “nova” e “por iniciativa própria”. A L-117 mantém a ressalva. (app/docs/LEIS.md:138) |
| Decidir quando uma **obra registrada** entra como complemento de outra. (pai:CLAUDE.md:481–482) | Decidir quando um **achado** entra como complemento. (novo:CLAUDE.md:461–462) | Troca o objeto da delegação. |
| DIFF **antecipado** também na troca de motor. (pai:CLAUDE.md:563–567) | DIFF da troca uma vez como impacto, sem “antes”. (novo:CLAUDE.md:594–595) | Perde o prazo explícito, embora preserve finalidade e frequência. |

Há ainda uma **ambiguidade nova no B3**: “reprovacao do verificador final” entra como gatilho sem explicitar a **terceira** reprovação. O código referenciado exige três; portanto, a referência ao B3 permite recuperar o limiar, mas o enunciado isolado parece abranger qualquer reprovação. (novo:CLAUDE.md:458–459; bin/disjuntor.py:11–12, 68)

3. **Scripts que realmente leem estrutura ou conteúdo do `CLAUDE.md`**

| Script | O que procura | Efeito do diff |
|---|---|---|
| `bin/tests/test_lei_akita.sh` | Título `## 0. LEI-AKITA`, recorte até seção 1, 13 itens numerados e frase `LEI-AKITA: origem=`. (bin/tests/test_lei_akita.sh:28–31) | Alvos preservados. (novo:CLAUDE.md:14, 45, 63, 78) |
| `bin/tests/test_suite_sh.sh` | Título literal `## 3. TESTES - COMANDO CANONICO`, comando seguinte e presença de `bin/suite.sh`. (bin/tests/test_suite_sh.sh:22–32, 37–42) | Título e comando preservados. (novo:CLAUDE.md:170–171) |
| `bin/tests/test_segredo_fora_do_argv.sh` | Sequência proibida `-e DB_PASSWORD=`, com filtro para linhas contendo crase. (bin/tests/test_segredo_fora_do_argv.sh:19, 33) | A busca continua aplicável; não depende dos títulos ou das regras fundidas da 7b. (bin/tests/test_segredo_fora_do_argv.sh:33) |
| `bin/tests/test_claude_md_regras_preservadas.sh` | Nomes de regra/`L-NNN`, referências a LAPIDES, nomes e âncoras da PISTA, hashes dos blocos da PISTA. (bin/tests/test_claude_md_regras_preservadas.sh:43–77) | Os apelidos e ponteiros continuam presentes; o `.ref` acrescenta `PENDENTE-SAI` e `PRE-VERIFICADOR`. (novo:CLAUDE.md:436–443, 477–482, 514–520, 603–606; novo:bin/tests/claude_md_regras.ref:76, 85) |

**Nenhuma dependência textual desses leitores ficou sem alvo pela fusão**, conforme comparação estática dos predicados e documentos citados; execução integral dos selos: **[não verificado]**.

O selo de preservação **não prova equivalência semântica**: no `CLAUDE.md`, confere nomes e referências; a comparação literal por hash incide sobre `PISTA.md`. (bin/tests/test_claude_md_regras_preservadas.sh:57, 69–77)

Não entram nessa lista os testes de mera existência em `raia_agente.sh` e `test_raia_declarada_no_estado.sh`, nem o reconhecimento do nome de anexo no disjuntor. (bin/raia_agente.sh:29; bin/tests/test_raia_declarada_no_estado.sh:17; bin/disjuntor.py:772, 886–888)

4. **Sequência das letras e item (5)**

O trecho removido do **CLAUDE.md não trazia essa enumeração**: era um resumo com ponteiro para a PISTA, seguido da regra MODO-COMUM, classe `(d)`. (pai:CLAUDE.md:508–509)

No texto literal referido, a triagem já enumerava **`(a), (b), (c), (e)`**; o bloco MODO-COMUM seguinte definia **`(d)`**. O novo reúne essas cinco classes como **`(a), (b), (c), (e), (d)`**. **Nenhuma letra desapareceu.** (pai:app/docs/PISTA.md:122–125, 133–138; novo:CLAUDE.md:557–559)

O salto de **item `(4)` para `(6)`**, sem `(5)`, **já existia na PISTA**. Não foi uma supressão deste diff. Separadamente, o novo CODEX tem item **5, PRE-VERIFICADOR**. (pai e novo:app/docs/PISTA.md:128–130; novo:CLAUDE.md:544)

## 31/10 13:24 · sessao 49472567559f · F1C4-3-EMISSORES · verificador · origem=Code

**Há bloqueios na obra:** a guarda permite pergunta nova com `trabalha=True` em competência exportada; o lote pode contornar inclusive o desvio do limbo para Pauta; e a idempotência depende de marcadores que `escrever` ainda pode truncar. (`app/chamados/services/fabrica_da_celula.py:153–160`; `app/chamados/management/commands/disparar_perguntas_competencia.py:277–289`; `app/pautas/services.py:168–177`)

Auditoria estática, somente leitura. Não executei testes; resultados dos mutantes e ocorrência dos defeitos na frota são **[não verificado]**. [nao verificado]

**1. Escritores e leitores**

O mapa abaixo acompanha as portas, fontes e consumidores da cadeia alcançada pelo delta; os leitores que ainda decidem por conta própria estão discriminados no item 2. (`app/chamados/services/fabrica_da_celula.py:75–126`; `app/chamados/services/materializacao.py:156–168`; `app/ponto/services/cluster_espurio.py:23–35`; `app/ponto/management/commands/detectar_intervalo_ausente.py:46–87`)

| Campo/entidade | Quem escreve | Quem lê |
|---|---|---|
| `Pauta.texto` | O novo emissor monta marcador e tags; `pautas.services.escrever` acrescenta contexto, trunca e cria. (`app/chamados/services/fabrica_da_celula.py:185–190`; `app/pautas/services.py:163–177`) | O emissor procura prefixo e tags para deduplicar; a view entrega texto ao usuário. (`app/chamados/services/fabrica_da_celula.py:180–182`; `app/pautas/views.py:39–48`) |
| `Pauta.ancora_tipo` | O emissor passa `ANCORA_DIA`; `escrever` valida e persiste. (`app/chamados/services/fabrica_da_celula.py:173–186`; `app/pautas/services.py:158`; `app/pautas/services.py:176`) | Deduplicação, contexto do dia, chip por âncora e payload da view. (`app/chamados/services/fabrica_da_celula.py:180`; `app/pautas/services.py:168`; `app/pautas/services.py:328`; `app/pautas/views.py:40`) |
| `Pauta.ancora_id` | O emissor calcula `colaborador_id:AAAA-MM-DD`; `escrever` persiste como string. (`app/chamados/services/fabrica_da_celula.py:175`; `app/pautas/services.py:176`) | Deduplicação, resolução do contexto, chip e view. (`app/chamados/services/fabrica_da_celula.py:180`; `app/pautas/services.py:401`; `app/pautas/services.py:328`; `app/pautas/views.py:41`) |
| `Pauta.pai` | `escrever` recebe `pai=None`; `responder` cria outra Pauta com `pai=pauta`, conservando a âncora. O novo emissor omite `pai`, portanto cria cabeça. (`app/pautas/services.py:155`; `app/pautas/services.py:177`; `app/pautas/services.py:184–192`; `app/chamados/services/fabrica_da_celula.py:185–190`) | A deduplicação nova exige cabeça; a view lê filhas por `respostas`; `pautas_do_colab` também seleciona cabeças. (`app/chamados/services/fabrica_da_celula.py:180`; `app/pautas/views.py:46–48`; `app/pautas/services.py:425`) |
| `Pauta.feito_em` | `marcar_feita` e `marcar_feita_pelo_sistema`. (`app/pautas/services.py:239–240`; `app/pautas/services.py:264–265`) | `vivas`, guardas das operações e apresentação do estado. **O novo deduplicador não filtra esse campo.** (`app/pautas/models.py:57–58`; `app/pautas/services.py:236`; `app/pautas/services.py:282`; `app/pautas/views.py:43–58`; `app/chamados/services/fabrica_da_celula.py:180–181`) |
| `Pauta.cancelado_em` | `cancelar`. (`app/pautas/services.py:286`) | `nao_canceladas`/`vivas`, listagem e contadores. **O novo deduplicador não filtra esse campo.** (`app/pautas/models.py:57–64`; `app/pautas/services.py:315`; `app/pautas/services.py:340`; `app/chamados/services/fabrica_da_celula.py:180–181`) |
| `PerguntaDisputa` | A fábrica chama `criar_ou_atualizar_disputa` e `gerar_perguntas_disputa`; a geração chama `PerguntaDisputa.abrir`, que reutiliza/reaponta a existente ou cria. (`app/chamados/services/fabrica_da_celula.py:124–126`; `app/chamados/services/disputa_emissao.py:389–397`; `app/chamados/models.py:2408–2454`) | A fábrica lê motivo, identidade e carimbo; o estado da pergunta consulta `pergunta_viva`; a materialização usa pergunta/disputa na guarda de plantio. (`app/chamados/services/fabrica_da_celula.py:103–110`; `app/chamados/models.py:2476–2483`; `app/chamados/services/materializacao.py:807–824`) |
| Carimbo de `PerguntaDisputa` | No caminho já existente da fábrica, `desfazer_carimbo_sem_lastro` relê sob trava, consulta `pergunta_viva` e limpa os campos de materialização. Essa operação ocorre **antes** do novo gate de exportada. (`app/chamados/services/fabrica_da_celula.py:145–155`; `app/chamados/services/relavra_via.py:217–235`) | A coleta de candidatas e o próprio juiz da porta. (`app/chamados/services/fabrica_da_celula.py:103–110`; `app/chamados/services/relavra_via.py:223–227`) |
| `ChamadoColaborador` | A disputa abre chamado pai por `abrir`; pergunta nova órfã aciona `reatar_pergunta_orfa`; cluster abre por `abrir` e fecha por `reconciliar`. (`app/chamados/services/disputa_emissao.py:504–519`; `app/chamados/services/disputa_emissao.py:404–410`; `app/ponto/services/cluster_espurio.py:83–105`) | Cluster lê vivos para reconciliação e qualquer estado para dedup; intervalo lê vivos legados. O gate lê existente após travar o colaborador. (`app/ponto/services/cluster_espurio.py:45–59`; `app/ponto/management/commands/detectar_intervalo_ausente.py:123–138`; `app/chamados/models.py:861–865`) |
| `CelulaDia.trabalha` | A porta da célula calcula pelo vínculo e cria/regenera; fora da vigência escreve `False`; reversão deriva do DNA; geração em período cria/atualiza. (`app/ponto/portas/celula.py:231–264`; `app/ponto/portas/celula.py:532–538`; `app/ponto/portas/celula.py:590–615`) | `dia_sem_trabalho`; o gate de exportada lê diretamente `is None`; o leitor da grade usa como fallback. Os quatro emissores consultam o juiz. (`app/chamados/juizes.py:408`; `app/chamados/services/fabrica_da_celula.py:153`; `app/escala/services/leitor_celula.py:344–353`; `app/chamados/services/fabrica_da_celula.py:80`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`) |
| `ata['tipo_dia']` | A grade produz trabalho/folga e aplica overlays de falta/ausência; `ata_do_dia` copia; `lavrar_veredito` persiste a ata pela porta. (`app/escala/utils.py:727–752`; `app/escala/utils.py:1118`; `app/escala/utils.py:1174–1180`; `app/escala/utils.py:1743–1749`; `app/ponto/services/cartorio.py:567`; `app/ponto/portas/celula.py:730–743`; `app/ponto/portas/celula.py:767–775`) | Juiz, projetor da grade e veto repetido em `_marcos_faltantes`. (`app/chamados/juizes.py:408`; `app/escala/services/leitor_celula.py:344`; `app/chamados/services/declaracao_texto.py:90–96`) |
| `ata['sem_ancora']` | Há três fontes da grade: `work is None`, `_sem_anc_i` no intermitente e `eh_dia_trabalho(...) is None` no não intermitente; cartório copia e a porta persiste. (`app/escala/utils.py:729`; `app/escala/utils.py:1514`; `app/escala/utils.py:1701–1702`; `app/escala/utils.py:1752`; `app/ponto/services/cartorio.py:570`; `app/ponto/portas/celula.py:768`) | O projetor copia; folha usa como causa e fechamento conta dias incertos. Cluster e intervalo deixam de usá-la como veto; o juiz não a lê. (`app/escala/services/leitor_celula.py:362`; `app/folha/causas.py:96–99`; `app/ponto/services/fechamento.py:1018`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; `app/chamados/juizes.py:407–408`) |
| `_C18` | É declaração estática em `core/juizes.py`, associada ao endereço de `dia_sem_trabalho`; a documentação e o corte acompanham o registro. (`app/core/juizes.py:142`; `app/core/juizes.py:266`; `app/docs/JUIZES.md:55–57`; `app/docs/CORTES.md:415`) | O gerador de diagrama lê `JUIZES`; a trava de juiz novo lê os endereços e procura o corte. Os emissores importam a função diretamente. (`app/core/management/commands/gerar_diagrama.py:341–348`; `bin/tests/test_juiz_novo_tem_corte.sh:27–44`; `app/chamados/services/fabrica_da_celula.py:78`; `app/chamados/services/materializacao.py:141`; `app/ponto/services/cluster_espurio.py:20`; `app/ponto/management/commands/detectar_intervalo_ausente.py:37`) |

**2. Juiz único e respostas paralelas**

O predicado real é:

```python
cel.trabalha is False or bool(tipo_dia and tipo_dia != 'trabalho')
```

Logo, `None` com tipo ausente ou `'trabalho'` não cala; tipo não vazio diferente de trabalho cala; `sem_ancora` não participa. (`app/chamados/juizes.py:407–408`)

Não encontrei outra implementação de C18 dentro de `core/juizes.py`: ali estão pergunta e endereço. O juiz do vínculo responde previsão, e o de status responde status lavrado; não são duplicações da mesma pergunta. `_celula_furada` também não duplica: chama `dia_sem_trabalho`. (`app/core/juizes.py:142`; `app/core/juizes.py:165`; `app/core/juizes.py:180`; `app/core/juizes.py:266`; `app/chamados/juizes.py:443`)

Persistem estes sítios de produção:

| Sítio | Decisão própria e diferença para C18 |
|---|---|
| `declaracao_texto._marcos_faltantes` | Elimina marcos por `tipo_dia != 'trabalho'`. Não é equivalente ao juiz: não incorpora a soberania de `trabalha=False` com ata contraditória; tipo indefinido também é descartado. (`app/chamados/services/declaracao_texto.py:90–96`) |
| `supra_juiz.classificar_dia` | Produz `COBRANCA_EM_DIA_SEM_TRABALHO` com tipo diferente de trabalho, fora de `SEM_JURISDICAO`, previsto zero, chamado vivo e nenhuma cobertura. É predicado diferente, mais condicionado; a migração está declarada no corte, mas não realizada nesse sítio. (`app/ponto/supra_juiz.py:443–447`; `app/docs/CORTES.md:415`) |
| `marcos.celula_veta_cobranca` | Decide `FOLGA` por `trabalha is False`, sem consultar a cláusula da ata; acrescenta vetos de veredito. (`app/ponto/marcos.py:155–162`) |
| Consumidores desse veto paralelo | Detector de ausências, emissor da grade, reconciliador de pergunta órfã e leitor de furos sem cobrança continuam usando-o. (`app/ponto/management/commands/detectar_ausencias.py:625–628`; `app/ponto/management/commands/reconciliar_grade.py:502–504`; `app/chamados/reconciliador.py:768`; `app/ponto/services/furos_sem_cobranca.py:98`) |
| Primeiro universo do lote de perguntas | Seleciona somente `trabalha=True`, excluindo limbo antes de consultar o juiz. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`) |
| Vigia de hora | Também seleciona somente `trabalha=True`, deixando o limbo fora desse caminho. (`app/ponto/services/vigia_de_hora.py:21–23`) |
| Worklist | Classifica qualquer tipo diferente de trabalho como dia sem gap desenhável. (`app/ponto/worklist.py:19–20`) |
| Relatório de furos e seus auxiliares | Excluem tipo diferente de trabalho e também `sem_ancora`, inclusive nos auxiliares usados pelo Raio-X. (`app/relatorios/furos.py:151–159`; `app/relatorios/furos.py:218–219`; `app/relatorios/furos.py:237–238`) |
| Explicação do dia para admin/IA | `not cel.trabalha` afirma que nenhuma marca era esperada, alcançando também `None`. (`app/ponto/services/dia_do_colab.py:121–123`) |
| Relatório diário da frota | `not cel.trabalha` conta `None` como folga e não chega à lista dos dias que acusam. (`app/relatorios/dia_da_frota.py:47–55`) |
| Vigia de entrada | Seus dois universos usam `trabalha=True`, excluindo limbo da contagem de lâmpadas. (`app/ponto/services/vigia_entrada.py:78–85`; `app/ponto/services/vigia_entrada.py:120–132`) |

As últimas cinco linhas são testemunhas/contadores, não novas portas de nascimento; ainda assim mantêm critérios próprios para apresentar ou excluir o dia. (`app/ponto/worklist.py:7–26`; `app/relatorios/furos.py:151–164`; `app/ponto/services/dia_do_colab.py:113–123`; `app/relatorios/dia_da_frota.py:47–57`; `app/ponto/services/vigia_entrada.py:78–85`)

**3. Históricos de Pauta e reescrita**

A identidade efetivamente usada é: **âncora exata + destinatário DP + cabeça + prefixo do marcador + tag completa do motivo**. Não entram remetente, autor, `feito_em`, `cancelado_em` ou existência de resposta. (`app/chamados/services/fabrica_da_celula.py:175–183`)

| Histórico encontrado | Escreve novamente? |
|---|---|
| Cabeça viva com prefixo e tag completa | **Não**, para aquele motivo. (`app/chamados/services/fabrica_da_celula.py:180–183`) |
| Cabeça feita com prefixo e tag completa | **Não**: não existe filtro por `feito_em`. (`app/chamados/services/fabrica_da_celula.py:180–181`) |
| Cabeça cancelada com prefixo e tag completa | **Não**: não existe filtro por `cancelado_em`. (`app/chamados/services/fabrica_da_celula.py:180–181`) |
| Cabeça respondida, com filho | **Não**, se a cabeça conserva a tag; resposta não altera essa consulta. (`app/pautas/services.py:184–192`; `app/chamados/services/fabrica_da_celula.py:180–182`) |
| Motivo consta somente no filho | **Sim**, se nenhuma cabeça elegível contém a tag: filhos são excluídos. (`app/chamados/services/fabrica_da_celula.py:180–183`) |
| Cabeça contém apenas parte dos motivos pedidos | Escreve uma Pauta com os motivos restantes. (`app/chamados/services/fabrica_da_celula.py:182–190`) |
| Texto truncado, mas prefixo e tag sobreviveram completos | **Não**, mesmo que a explicação tenha sido cortada. (`app/chamados/services/fabrica_da_celula.py:180–183`; `app/pautas/services.py:171–177`) |
| Prefixo sobreviveu, mas uma tag foi cortada | Escreve novamente os motivos cujas tags completas não foram encontradas. (`app/chamados/services/fabrica_da_celula.py:181–190`) |
| O próprio prefixo foi cortado | Aquela Pauta não participa da deduplicação; os motivos podem ser escritos novamente. (`app/chamados/services/fabrica_da_celula.py:180–183`; `app/pautas/services.py:171`) |

**Mover as tags para o começo não resolve todo truncamento.** `escrever` reserva espaço para um contexto cujo tamanho inclui nome e todas as batidas; o orçamento do corpo é `500-len(_ctx)-4`. Esse orçamento pode cortar marcador/tag. Se a tag continuar sem caber nas tentativas seguintes, a repetição pode continuar. É contraexemplo permitido pelo código; ocorrência na frota é **[não verificado]**. (`app/pautas/services.py:168–177`; `app/pautas/services.py:413–420`; `app/chamados/services/fabrica_da_celula.py:180–190`)

Há também falta de proteção concorrente: duas chamadas podem ler ausência antes de ambas escreverem. A leitura e a escrita não são serializadas pelo helper; o `atomic` de `escrever` começa depois da consulta e o modelo não declara unicidade desse fato. Duplicação concorrente efetivamente ocorrida é **[não verificado]**. (`app/chamados/services/fabrica_da_celula.py:180–185`; `app/pautas/services.py:152–157`; `app/pautas/models.py:123–132`)

**4. Mutação mínima por teste** [nao verificado]

A tabela prevê o resultado por inspeção; morte efetiva dos mutantes é **[não verificado]**. Os nomes abaixo omitem somente o prefixo `test_`. (`app/chamados/tests/test_f1c4_3_emissores.py:80–153`; `app/chamados/tests/test_f1c4_3_emissores.py:199–239`)

| Teste | Mutação de produção que o deixa vermelho | [nao verificado]
|---|---|
| `RED_fabrica_limbo_pergunta` — `app/chamados/tests/test_f1c4_3_emissores.py:80` | Restaurar `cel.trabalha is not True` no veto. (`app/chamados/services/fabrica_da_celula.py:80`) |
| `fabrica_folga_nao_pergunta` — `app/chamados/tests/test_f1c4_3_emissores.py:83` | Trocar o retorno do veto por `['orfao_14h']`. Remover somente o juiz **não** basta, pelo segundo veto. (`app/chamados/services/fabrica_da_celula.py:81`; `app/chamados/services/declaracao_texto.py:95–96`) |
| `MORDE_fabrica_dia_de_trabalho_pergunta` — `app/chamados/tests/test_f1c4_3_emissores.py:86` | Tornar o veto incondicional. (`app/chamados/services/fabrica_da_celula.py:80–81`) |
| `fabrica_ausencia_lavrada_com_trabalha_true_nao_pergunta` — `app/chamados/tests/test_f1c4_3_emissores.py:89` | Trocar o retorno do veto por lista não vazia. Remover só a cláusula da ata do juiz **sobrevive**, pelo segundo veto. (`app/chamados/services/fabrica_da_celula.py:81`; `app/chamados/services/declaracao_texto.py:95–96`) |
| `RED_materializacao_limbo_furo_absolve_o_plantio` — `app/chamados/tests/test_f1c4_3_emissores.py:100` | Restaurar `cel.trabalha is not True`. (`app/chamados/services/materializacao.py:160`) |
| `materializacao_folga_nao_absolve` — `app/chamados/tests/test_f1c4_3_emissores.py:103` | Remover o juiz do veto, deixando somente `if not lamp`. (`app/chamados/services/materializacao.py:160–168`) |
| `MORDE_materializacao_dia_de_trabalho_absolve` — `app/chamados/tests/test_f1c4_3_emissores.py:106` | Tornar o veto incondicional. (`app/chamados/services/materializacao.py:160`) |
| `RED_materializacao_ausencia_lavrada_com_trabalha_true_nao_absolve` — `app/chamados/tests/test_f1c4_3_emissores.py:109` | Substituir o juiz por `cel.trabalha is False`. (`app/chamados/services/materializacao.py:160`) |
| `RED_cluster_limbo_conta` — `app/chamados/tests/test_f1c4_3_emissores.py:118` | Restaurar `cel.trabalha is not True`. (`app/ponto/services/cluster_espurio.py:28`) |
| `cluster_folga_nao_conta` — `app/chamados/tests/test_f1c4_3_emissores.py:121` | Usar somente `cel.trabalha is False`; a fixture é `None/folga`. (`app/ponto/services/cluster_espurio.py:28`) |
| `MORDE_cluster_dia_de_trabalho_conta` — `app/chamados/tests/test_f1c4_3_emissores.py:124` | Tornar o veto incondicional. (`app/ponto/services/cluster_espurio.py:28`) |
| `RED_cluster_ausencia_lavrada_com_trabalha_true_nao_conta` — `app/chamados/tests/test_f1c4_3_emissores.py:127` | Usar somente `cel.trabalha is False`. (`app/ponto/services/cluster_espurio.py:28`) |
| `cluster_sem_ancora_com_trabalha_true_conta` — `app/chamados/tests/test_f1c4_3_emissores.py:130` | Reintroduzir `or ata.get('sem_ancora')`. (`app/ponto/services/cluster_espurio.py:28`) |
| `RED_intervalo_limbo_conta` — `app/chamados/tests/test_f1c4_3_emissores.py:140` | Restaurar `cel.trabalha is not True`. (`app/ponto/management/commands/detectar_intervalo_ausente.py:51`) |
| `intervalo_folga_nao_conta` — `app/chamados/tests/test_f1c4_3_emissores.py:143` | Usar somente `cel.trabalha is False`. (`app/ponto/management/commands/detectar_intervalo_ausente.py:51`) |
| `MORDE_intervalo_dia_de_trabalho_conta` — `app/chamados/tests/test_f1c4_3_emissores.py:146` | Tornar o veto incondicional. (`app/ponto/management/commands/detectar_intervalo_ausente.py:51`) |
| `RED_intervalo_ausencia_lavrada_com_trabalha_true_nao_conta` — `app/chamados/tests/test_f1c4_3_emissores.py:149` | Usar somente `cel.trabalha is False`. (`app/ponto/management/commands/detectar_intervalo_ausente.py:51`) |
| `intervalo_sem_ancora_com_trabalha_true_conta` — `app/chamados/tests/test_f1c4_3_emissores.py:152` | Reintroduzir `or ata.get('sem_ancora')`. (`app/ponto/management/commands/detectar_intervalo_ausente.py:51`) |
| `RED_limbo_em_exportada_nao_pergunta_e_vai_a_pauta_dp` — `app/chamados/tests/test_f1c4_3_emissores.py:199` | Desativar o bloco de desvio para Pauta. (`app/chamados/services/fabrica_da_celula.py:153–157`) |
| `limbo_em_aberta_pergunta_sem_pauta` — `app/chamados/tests/test_f1c4_3_emissores.py:207` | Negar o resultado de `competencia_do_dia_entregue`. (`app/chamados/services/fabrica_da_celula.py:155`) |
| `MORDE_trabalho_em_exportada_segue_perguntando_sem_pauta` — `app/chamados/tests/test_f1c4_3_emissores.py:212` | Retirar `and cel.trabalha is None`, desviando também `True`. **Esse teste protege comportamento contrário ao aval literal (a).** (`app/chamados/services/fabrica_da_celula.py:153`; `app/chamados/tests/test_f1c4_3_emissores.py:158–165`) |
| `RED_limbo_em_exportada_duas_vezes_uma_pauta` — `app/chamados/tests/test_f1c4_3_emissores.py:218` | Remover `ja.update(...)`; a segunda passagem escreve outra. (`app/chamados/services/fabrica_da_celula.py:182–185`) |
| `RED_limbo_em_exportada_pauta_feita_nao_reabre` — `app/chamados/tests/test_f1c4_3_emissores.py:227` | Acrescentar `feito_em__isnull=True` à consulta histórica. (`app/chamados/services/fabrica_da_celula.py:180`) |

Nos dois negativos da fábrica, trocar o retorno por lista é uma mutação mínima de resultado, mas **não prova a costura com o juiz**. Para liberar folga/ausência pelo fluxo normal, é preciso retirar os dois vetos: o da fábrica e o d

[truncado em 20000 de 24308 caracteres]

## 31/10 13:23 · sessao a7e474fec9ef · - · - · origem=Ronald (tmux)

Contraprova: há perdas e ambiguidades textuais; expansões já existentes na PISTA não devem ser tratadas como regras novas, pois ela continua sendo a referência em caso de divergência. (`app/docs/PISTA.md:8–12`)

| Ponto | Pai `41b4d89d` | Novo `e25a0940` |
|---|---|---|
| Obrigação positiva de exibir número | Sem fechamento, **MOSTRAR o número do motor rotulado**, evitando zero. (`CLAUDE.md:520–521`) | Preserva apenas “0 leitor mostrando número […] sem o rótulo”; isso não obriga mostrar o número e permite passar ocultando-o. (`CLAUDE.md:567–570`) |
| Antecipação do DIFF da troca de motor | DIFF **ANTECIPADO** sobrevive em exportada **e troca de motor**. (`CLAUDE.md:563–567`) | ANTES aparece na exportada; troca de motor conserva uma vez/impacto, sem prazo. (`CLAUDE.md:582–583`, `CLAUDE.md:594–595`) |
| Sonnet lendo código | Só rc LIMITE 75 ou falha, com motivo no RELATO. (`CLAUDE.md:506`; `app/docs/PISTA.md:92–93`) | “leitura/sonda” admite também “ou motivo na linha do DESPACHO”; estende à leitura uma exceção antes expressa para sonda. (`CLAUDE.md:532–534`; antecedente da exceção: `app/docs/PISTA.md:159–160`) |
| Juiz do prazo existente | Deve responder à **MESMA pergunta**. (`CLAUDE.md:659–660`) | Basta “se já houver”; perde a correspondência da pergunta. (`CLAUDE.md:623–624`) |
| Registro do juiz do prazo | Escrever a frase **e declarar em `JUIZES_DO_PRAZO`**. (`CLAUDE.md:661–662`) | Conserva frase, referência e número intocado; declaração sem destino. (`CLAUDE.md:624–625`) |
| B3 | Código dispara na **3ª reprovação final**. (`bin/disjuntor.py:11`, `bin/disjuntor.py:68`, `bin/disjuntor.py:574`) | “reprovação do verificador final (DISJUNTOR B3)” sem limiar pode ser lida como qualquer reprovação; conflita com o retorno ao ciclo previsto no próprio CODEX. (`CLAUDE.md:458–459`, `CLAUDE.md:550`) |

Outras omissões da consolidação: “citando este aval” na declaração de autoridade já existente desaparece, embora continue na PISTA; moratória perde no texto a métrica/juiz e o qualificador “por iniciativa própria”, conservados na L-117 referenciada. (pai `CLAUDE.md:483`, novo `CLAUDE.md:462–464`, `app/docs/PISTA.md:53`; pai `CLAUDE.md:644–647`, novo `CLAUDE.md:455`, `app/docs/LEIS.md:138`)

Sobre a pergunta 4: o trecho removido do **CLAUDE** não enumerava as classes, apenas remetia à PISTA. A PISTA já trazia `(a),(b),(c),(e)` na triagem e `(d)` no bloco MODO-COMUM seguinte; o novo reúne essas classes em `(a),(b),(c),(e),(d)`. Nenhuma letra desapareceu. (pai `CLAUDE.md:508–509`; pai `app/docs/PISTA.md:122–125`, `app/docs/PISTA.md:133–138`; novo `CLAUDE.md:557–559`)

O salto de `(4)` para `(6)`, sem `(5)`, **já existia** no texto literal da PISTA, mantido neste commit; não representa cláusula apagada por esta fusão. (pai e novo `app/docs/PISTA.md:128–130`)

## 31/10 13:23 · sessao 964b0c846f95 · - · - · origem=Ronald (tmux)

Use **P = `41b4d89d:CLAUDE.md`** e **N = `e25a0940:CLAUDE.md`**.

Omissões verificadas dos consolidados:

| Cláusula removida | Destino novo / falta |
|---|---|
| “sem fechamento, MOSTRAR o numero do motor rotulado — zero seria perda de informacao” (P:520–521) | N:567–570 mantém os dois zeros do placar, mas perde a obrigação positiva de mostrar o número; **sem destino**. |
| Frase de juiz no CORTES “**citando este aval**” (P:483) | N:462–464 mantém condições medidas, mas omite a citação. Ainda preservada em `app/docs/PISTA.md:52–53`. |
| Handoff obrigatório contendo HEAD/origin, status, ORDEM VIVA/item em curso **lidos de hook_stop_fila1**, PAREI, avais, processos e próximo passo (P:610–612) | N:511–512 mantém comando, teto 60 e hooks; **sem destino para conteúdo e leitor canônico**. |
| “o pendente VOLTA se o contador sair do esperado” (P:653–654) | N:621 diz apenas “frase de reversao”; omite o gatilho e a consequência explícitos. Ainda existe em `app/docs/LEIS.md:139`. |
| Novo juiz de prazo deve ser declarado em **JUIZES_DO_PRAZO** (P:661–662) | N:624–626 mantém frase no CORTES e número intocado; **sem destino para a inscrição no registro**. |
| “backup e renomeacao sao do admin” (P:535) | N:589–593 mantém nome e versões; **sem destino para a responsabilidade do admin**. |
| “tarefa so por delegacao do Claude” (P:504) | N:521–528 exige wrapper, mas omite quem delega. Ainda existe em `app/docs/PISTA.md:72–74`. |
| Sem critério escrito no aval, vale L-009 (P:579) | N:586–588 mantém condições e parada, mas **sem destino explícito para esse fallback normativo**. |
| Moratória: juiz que determina `contratos_estruturais=20/20`, corte literal Ronald e extinção automática (P:644–647) | N:455 reduz a “Ate 20/20 ... (L-117)”; detalhes não aparecem nos consolidados, embora preservados em `app/docs/LEIS.md:138`. |

Mudanças textuais concretas:

| Antes | Depois | Efeito |
|---|---|---|
| Classe nova de bug muda desenho: topo **PENDENTES** (P:459–460) | Topo **RELATO**, “PERGUNTA DE DESENHO com numeros”, obra para/outras seguem; inclui reprovação final B3 (N:458–459) | Troca destino e amplia gatilho. B3 real é **3ª** reprovação (`bin/disjuntor.py:11`, `:68`); consolidação não traz esse número. |
| Sonnet leitura só LIMITE75/falha, com motivo (P:506) | “leitura/sonda” aceita também escrita na sombra **ou motivo no DESPACHO** (N:533–534) | Amplia exceção de leitura. Permissão por motivo já existia para **sonda**, em `app/docs/PISTA.md:159–160`; restrição da leitura permanece em `app/docs/PISTA.md:92–93`. |
| Juiz de prazo precisa responder à **MESMA pergunta** (P:659–660) | Pergunta a juiz de JUIZES_DO_PRAZO “**se ja houver**” (N:623–624) | Troca correspondência da pergunta por mera existência textual. |
| Não propor/numeração de lei **nova por iniciativa própria** (P:645) | Não numera nem propõe **lei** (N:455) | Texto fica mais restritivo, omitindo a ressalva e o nascimento por corte literal. |
| Quando **obra registrada** entra como complemento de outra (P:481–482) | Quando **achado** entra como complemento (N:461–462) | Muda o objeto da delegação; destino de achado de esmeril segue enumerado separadamente em N:462. |

Não tratar as reexpansões do PISTA como inovação: `app/docs/PISTA.md:8–10` já declara texto literal e prevalência sobre o resumo; os cinco consolidados mantêm seus ponteiros (N:481, N:519–520). A sequência de letras e o salto numérico também já constavam lá: classificação `(a),(b),(c),(e)` em `app/docs/PISTA.md:122–125`, classe `(d)` em `:133–138`, itens `(1),(2),(3),(4),(6)` em `:120–130`; **não havia item (5) para perder** nessas regras.

## 31/10 13:22 · sessao cfac98e23723 · REPROVA-E-DESENHO · verificador · origem=Code

**Reprovo a garantia de bloqueio desta volta:** há caminhos que permitem novo verificador sem linha posterior ao disparo, e corridas que negam mesmo após uma liberação válida. O `flock` protege cada alteração do JSON, mas não a decisão completa `varrer → liberar → permitir`. (`bin/disjuntor.py:242`, `bin/disjuntor.py:616`, `bin/disjuntor.py:622`, `bin/disjuntor.py:1096`)

**1. Falhas contra o código e o aval**

- **A fotografia não prova quando a linha foi escrita.** `_topo_shas` transforma leitura malsucedida em `[]`; depois, qualquer escrita no RELATO pode atualizar seu `mtime`, tornando elegível uma linha antiga que ficou fora da fotografia. A checagem compara conteúdo e data do arquivo inteiro, não uma ocorrência de escrita da linha. (`bin/disjuntor.py:638`, `bin/disjuntor.py:643`, `bin/disjuntor.py:607`, `bin/disjuntor.py:609`)

- **Stop e PreToolUse não decidem sobre a mesma observação.** O Stop captura topo/mtime **antes** de varrer; a varredura fotografa novamente no disparo. Já o PreToolUse lê o topo depois da varredura. Assim, o Stop pode consumir uma candidata que já saiu do topo antes do disparo. (`bin/hook_stop_fila1.py:1044`, `bin/hook_stop_fila1.py:1045`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:700`, `bin/disjuntor.py:1096`)

- **A conferência de redisparo deixa outra janela.** `libera_b3` solta a trava, relê `disparado`, e depois registra `LIBERA` e retorna sucesso. Um redisparo entre essa releitura e o retorno permite autorizar o final com a obra novamente disparada. (`bin/disjuntor.py:272`, `bin/disjuntor.py:622`, `bin/disjuntor.py:624`, `bin/disjuntor.py:634`, `bin/disjuntor.py:635`)

- **Falha de gravação preserva somente a decisão daquela chamada.** O disparo é anunciado antes de persistir; se a gravação falhar, o retorno continua valendo, mas o disparo pode desaparecer na chamada seguinte. O consumo tem conferência posterior de persistência; o disparo não tem proteção equivalente. (`bin/disjuntor.py:257`, `bin/disjuntor.py:263`, `bin/disjuntor.py:269`, `bin/disjuntor.py:623`, `bin/disjuntor.py:701`)

- **A garantia absoluta também esbarra no comportamento herdado de falhar aberto.** Trava ocupada, estado ilegível ou erro durante a varredura podem chegar ao `except` que libera a ferramenta, inclusive havendo disparo persistido. O contrato local admite expressamente esse limite. (`bin/disjuntor.py:247`, `bin/disjuntor.py:254`, `bin/disjuntor.py:534`, `bin/disjuntor.py:1132`, `app/docs/RELATO.md:13`)

- **“Para a obra / antes de nova volta” foi implementado como “nega novo Agent verificador reconhecido como final”.** Edit, Write, Bash comum e novo construtor não recebem a trava B3; o contrato técnico documenta esse recorte, mais estreito que o aval registrado. (`bin/disjuntor.py:1075`, `bin/disjuntor.py:1093`, `bin/disjuntor.py:1096`, `app/docs/RELATO.md:11`, `app/docs/PROMPTS.md:1733`)

- **Há restrição adicional para erro bobo.** `ERRO-BOBO OBRA-X: faltou import` é recusado pelo mínimo de vinte caracteres; o aval pede uma linha de motivo. O mínimo aparece no contrato técnico, mas não no texto do aval registrado. (`bin/disjuntor.py:75`, `bin/disjuntor.py:581`, `app/docs/RELATO.md:12`, `app/docs/PROMPTS.md:1733`)

- **A validação comprova forma, não a pergunta sem juiz.** Uma linha com `pergunta=x app/inexistente.py:0` satisfaz o reconhecedor: ele exige uma palavra e uma referência com aparência de arquivo:linha, sem consultar existência do arquivo ou autoridade da pergunta. Isso limita o que o mecanismo efetivamente garante. (`bin/disjuntor.py:76`, `bin/disjuntor.py:576`, `bin/disjuntor.py:577`)

A linha única no CLAUDE e sua referência estão conformes ao recorte pedido. (`CLAUDE.md:502`, `bin/tests/claude_md_regras.ref:93`)

**2. Quem escreve e quem lê**

No código de produção, a porta física de persistência é `_com_estado`: lê, executa a transição e substitui o JSON sob `flock`. A leitura geral sem mutação é `ler_estado`. (`bin/disjuntor.py:229`, `bin/disjuntor.py:242`, `bin/disjuntor.py:257`, `bin/disjuntor.py:263`, `bin/disjuntor.py:275`)

| Campo | Quem escreve pela porta | Quem lê |
|---|---|---|
| `reprovados` | Varredura inicializa/acrescenta: `bin/disjuntor.py:690`, `bin/disjuntor.py:694`; liberação limpa: `bin/disjuntor.py:612`; migração limpa: `bin/disjuntor.py:662`; rearme limpa: `bin/disjuntor.py:719` | Liberação: `bin/disjuntor.py:611`; migração: `bin/disjuntor.py:658`, `bin/disjuntor.py:661`; varredura: `bin/disjuntor.py:693`, `bin/disjuntor.py:697`; rearme: `bin/disjuntor.py:717`, `bin/disjuntor.py:718` |
| `rearmado` | Migração: `bin/disjuntor.py:661`; rearme: `bin/disjuntor.py:718` | Migração: `bin/disjuntor.py:661`; varredura: `bin/disjuntor.py:691`; rearme: `bin/disjuntor.py:718` |
| `liberado` | Liberação: `bin/disjuntor.py:611` | Liberação: `bin/disjuntor.py:611`; varredura: `bin/disjuntor.py:691` |
| `disparado` | Varredura inicializa/dispara: `bin/disjuntor.py:690`, `bin/disjuntor.py:699`; liberação limpa: `bin/disjuntor.py:612`; rearme limpa: `bin/disjuntor.py:719` | Liberação: `bin/disjuntor.py:604`, `bin/disjuntor.py:624`; migração: `bin/disjuntor.py:655`; varredura: `bin/disjuntor.py:698`, `bin/disjuntor.py:705`; consulta: `bin/disjuntor.py:745` |
| `disparado_em` | Migração: `bin/disjuntor.py:657`; disparo: `bin/disjuntor.py:699` | Liberação: `bin/disjuntor.py:609`; migração: `bin/disjuntor.py:656` |
| `consumidas` | Liberação: `bin/disjuntor.py:613` | Elegibilidade/atualização/conferência: `bin/disjuntor.py:607`, `bin/disjuntor.py:613`, `bin/disjuntor.py:623` |
| `topo_no_disparo` | Migração: `bin/disjuntor.py:657`; disparo: `bin/disjuntor.py:700` | Liberação: `bin/disjuntor.py:607` |
| `b3_limiar` | Migração: `bin/disjuntor.py:663` | Guarda da migração e mensagem: `bin/disjuntor.py:651`, `bin/disjuntor.py:660` |

As transições são chamadas tanto pelo Stop quanto pelo PreToolUse; isso representa dois chamadores da mesma porta, não dois escritores físicos independentes. O rearme também usa essa porta. (`bin/hook_stop_fila1.py:1045`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:1096`, `bin/disjuntor.py:721`)

Todas as linhas abaixo passam pelo escritor `trilha`, que limita o detalhe a trezentos caracteres e engole falha da própria trilha. (`bin/disjuntor.py:120`, `bin/disjuntor.py:126`, `bin/disjuntor.py:127`)

| Linha | Quem escreve | Quem lê |
|---|---|---|
| `DISJUNTOR B3` | `varrer_b3 → dispara → trilha`: `bin/disjuntor.py:701`, `bin/disjuntor.py:133` | `ler` conta disparos: `bin/disjuntor.py:1180`, `bin/disjuntor.py:1202`; `linha` apresenta: `bin/disjuntor.py:1295` |
| `LIBERA` | Após conferência do consumo: `bin/disjuntor.py:634` | `ler` conta por tipo: `bin/disjuntor.py:1198`; devolve `b3_libera`: `bin/disjuntor.py:1238`; apresentação: `bin/disjuntor.py:1308` |
| `MIGRA` | Migração: `bin/disjuntor.py:659` | O selo procura: `bin/tests/test_disjuntor.sh:330`. Não entra nos eventos contabilizados por `ler`: `bin/disjuntor.py:1180` |
| `FALHA` | Registro no RELATO: `bin/disjuntor.py:224`; persistência: `bin/disjuntor.py:269`; consumo: `bin/disjuntor.py:626`, `bin/disjuntor.py:629`, `bin/disjuntor.py:632`; B4: `bin/disjuntor.py:1110`; PreToolUse: `bin/disjuntor.py:1133`; Stop: `bin/hook_stop_fila1.py:1056` | `ler` filtra essas linhas, mas só `relato_negacao` incrementa `sem_relato`: `bin/disjuntor.py:1180`, `bin/disjuntor.py:1196`, `bin/disjuntor.py:1197` |

A trilha não recompõe um disparo perdido: a decisão usa o estado e os transcripts; o leitor da trilha produz contadores. Portanto, `DISJUNTOR B3` ou `MIGRA` podem ser anunciados antes de uma gravação que falha, sem se tornarem estado durável. (`bin/disjuntor.py:257`, `bin/disjuntor.py:269`, `bin/disjuntor.py:659`, `bin/disjuntor.py:701`, `bin/disjuntor.py:1202`)

**3. Checklist das classes**

| Classe | O diff cai nela? |
|---|---|
| **Segundo juiz que duplica juiz registrado** | **Não demonstrado.** O reconhecedor duplicado do Stop foi removido; os dois caminhos usam `libera_b3`. Isso é instrumento de host. (`bin/hook_stop_fila1.py:411`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:1096`, `bin/disjuntor.py:4`) |
| **Segundo escritor fora da porta** | **Não demonstrado nas mutações B3.** Elas passam por `_com_estado`; a linha automática no RELATO passa por `no_relato`. A falha encontrada é de atomicidade entre operações. (`bin/disjuntor.py:616`, `bin/disjuntor.py:708`, `bin/disjuntor.py:721`, `bin/disjuntor.py:221`) |
| **Relógio lido fora da borda** | **Permanece como fallback nos auxiliares**, embora os dois hooks passem `agora`. A inconsistência efetiva é combinar esse instante com fotografias posteriores e `stat` separado do conteúdo. (`bin/disjuntor.py:653`, `bin/disjuntor.py:669`, `bin/disjuntor.py:738`, `bin/disjuntor.py:741`, `bin/hook_stop_fila1.py:1038`, `bin/hook_stop_fila1.py:1045`, `bin/disjuntor.py:1096`) |
| **Derivação paralela de fonte única** | **Não no reconhecedor:** identidade e topo foram compartilhados. Entretanto, a derivação de “escrita posterior” continua insuficiente, pelos casos temporais descritos. (`bin/disjuntor.py:570`, `bin/disjuntor.py:598`, `bin/disjuntor.py:733`, `bin/disjuntor.py:607`, `bin/disjuntor.py:609`) |
| **Selo/RED sem mordida** | **Há lacunas concretas.** O mutante de limiar 3 pode morrer na exigência textual `MIGRA ... -> 1`, antes de alcançar a primeira reprovação. Além disso, excluir `escritor unico` do predicado sobreviveria aos casos por inspeção: sua única utilização integrada espera negativa, não liberação. (`bin/tests/test_disjuntor.sh:330`, `bin/tests/test_disjuntor.sh:339`, `bin/tests/test_disjuntor.sh:845`, `bin/tests/test_disjuntor.sh:385`, `bin/tests/test_disjuntor.sh:387`, `bin/disjuntor.py:576`) |
| **Ordem de trava / Stop × PreToolUse** | **Sim.** Há captura anterior à varredura, operações separadas e conferência fora da trava. Isso permite tanto autorização indevida quanto negativa indevida. (`bin/hook_stop_fila1.py:1044`, `bin/disjuntor.py:604`, `bin/disjuntor.py:616`, `bin/disjuntor.py:622`, `bin/disjuntor.py:1096`) |
| **Pouso antes do pré-requisito** | **Não comprovado pelo diff.** Há exigência de pouso depois do produto, e o RELATO ainda registra produto que “não pousa”. Construção do instrumento não demonstra seu pouso efetivo. (`app/docs/PROMPTS.md:1733`, `app/docs/RELATO.md:4`, `app/docs/BACKLOG.md:779`) Pouso efetivo: **[não verificado]**. |

Outro sobrevivente possível por inspeção é trocar `>= B3_BOBO_MIN` por `> B3_BOBO_MIN`: os exemplos explícitos cobrem motivo curto e longo, sem testar exatamente vinte caracteres. (`bin/disjuntor.py:581`, `bin/tests/test_disjuntor.sh:696`, `bin/tests/test_disjuntor.sh:697`)

**4. Sequências de eventos**

**Sim, pode receber novo final sem linha válida escrita depois do disparo:**

1. **Fotografia ilegível → alarme renova o arquivo.** Existe linha antiga; o disparo fotografa `[]` por falha de leitura; uma negativa escreve o alarme preservando o topo e atualizando o RELATO; a próxima tentativa aceita a linha antiga pelo novo `mtime`. (`bin/disjuntor.py:643`, `bin/disjuntor.py:700`, `bin/disjuntor.py:1131`, `app/core/esteira_vigia.py:829`, `bin/disjuntor.py:607`, `bin/disjuntor.py:609`)

2. **Stop usa candidata anterior ao disparo.** Stop captura `agora=t0`; captura linha L e `mtime=t1≥t0`; outro escritor retira L; a varredura dispara e fotografa o topo sem L; Stop libera pela L que guardou. Nenhuma escrita de L depois desse disparo é necessária. (`bin/hook_stop_fila1.py:1038`, `bin/hook_stop_fila1.py:1044`, `bin/hook_stop_fila1.py:1045`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:607`, `bin/disjuntor.py:609`, `bin/disjuntor.py:700`)

3. **Liberação → conferência → redisparo → autorização.** A consome L e confere `disparado=False`; B registra nova reprovação e dispara; A retorna sucesso, permitindo o final sem linha posterior ao segundo disparo. (`bin/disjuntor.py:622`, `bin/disjuntor.py:624`, `bin/disjuntor.py:699`, `bin/disjuntor.py:635`, `bin/disjuntor.py:1096`)

4. **Disparo → gravação falha → troca de sessão.** A chamada anuncia o disparo, mas não o persiste; a nova sessão varre somente seus próprios subagentes e não encontra a reprovação anterior. Sem disparo no JSON, o novo final passa. (`bin/disjuntor.py:269`, `bin/disjuntor.py:319`, `bin/disjuntor.py:670`, `bin/disjuntor.py:680`, `bin/disjuntor.py:1096`)

5. **Disparo durável → falha na próxima consulta.** Trava ocupada ou transcript ilegível aborta a decisão; o tratamento geral libera a ferramenta. Esse é o limite herdado e declarado de falhar aberto. (`bin/disjuntor.py:247`, `bin/disjuntor.py:326`, `bin/disjuntor.py:534`, `bin/disjuntor.py:1133`, `app/docs/RELATO.md:13`)

**Sim, linha válida posterior também pode não liberar:**

1. **Outro hook já consumiu a linha.** PreToolUse observa disparo; Stop consome L; PreToolUse encontra `disparado=False`, recebe `None` de `libera_b3` e nega mesmo assim. (`bin/disjuntor.py:1096`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:604`, `bin/disjuntor.py:605`, `bin/disjuntor.py:1097`)

2. **Conteúdo novo com `mtime` antigo.** O `stat` ocorre; L é escrita; a leitura encontra L, mas a liberação recebe o `mtime` anterior e recusa. Uma tentativa posterior pode liberar. (`bin/disjuntor.py:738`, `bin/disjuntor.py:741`, `bin/disjuntor.py:609`)

3. **Migração redefine o disparo antigo.** A obra já estava disparada; L foi escrita depois daquele disparo; a migração estabelece `disparado_em=agora` e fotografa L. Ela passa a ser recusada pela fotografia e pela nova referência temporal. (`bin/disjuntor.py:655`, `bin/disjuntor.py:657`, `bin/disjuntor.py:607`, `bin/disjuntor.py:609`)

4. **Mesma linha reescrita após novo disparo.** O hash identifica conteúdo, não ocorrência; se já foi consumido, a reescrita continua recusada. Aqui a negativa é **intencional no contrato e no selo**, embora responda afirmativamente à pergunta temporal. (`bin/disjuntor.py:555`, `bin/disjuntor.py:607`, `app/docs/RELATO.md:12`, `bin/tests/test_disjuntor.sh:379`)

5. **Consumo não persistido.** Uma linha elegível não libera quando sua gravação no estado falha; a conferência mantém a negativa. Esse comportamento também é deliberado. (`bin/disjuntor.py:623`, `bin/disjuntor.py:628`, `bin/tests/test_disjuntor.sh:369`)

A suíte de host cria e apaga arquivos; sua execução integral e o resultado global dos mutantes ficam **[não verificado]** nesta auditoria de somente leitura. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:51`)

## 31/10 13:22 · sessao 0ad788016f50 · - · - · origem=Ronald (tmux)

Os **quatro leitores de conteúdo** encontrados no escopo pedido continuam encontrando seus alvos depois do diff; a checagem foi reproduzida em memória para os dois commits, sem executar os scripts que criam temporários. Os critérios estão em `bin/tests/test_lei_akita.sh:28`, `bin/tests/test_suite_sh.sh:22`, `bin/tests/test_segredo_fora_do_argv.sh:33` e `bin/tests/test_claude_md_regras_preservadas.sh:55`.

| Script | Dependência do conteúdo | Depois do diff |
|---|---|---|
| `bin/tests/test_lei_akita.sh` | Busca `^## 0. LEI-AKITA`, recorta até `^## 1\.`, exige 13 itens numerados e `LEI-AKITA: origem=`. `bin/tests/test_lei_akita.sh:28–31` | Preservados: título em `e25a0940:CLAUDE.md:14`, item 13 em `:45`, frase em `:63`, fim da seção em `:78`. |
| `bin/tests/test_suite_sh.sh` | Reconhece título literal `## 3. TESTES - COMANDO CANONICO`, extrai o comando seguinte e exige `bin/suite.sh`. `bin/tests/test_suite_sh.sh:22–32`, `:37–42` | Título e comando preservados em `e25a0940:CLAUDE.md:170–171`. |
| `bin/tests/test_segredo_fora_do_argv.sh` | Procura a sequência proibida `-e DB_PASSWORD=` e aplica filtro que ignora linhas contendo crase. `bin/tests/test_segredo_fora_do_argv.sh:19`, `:33` | Não há ocorrência acusável antes nem depois; não depende da seção 7b. Predicado em `bin/tests/test_segredo_fora_do_argv.sh:33`; resultado da comparação em memória [não verificado por execução do selo]. |
| `bin/tests/test_claude_md_regras_preservadas.sh` | Extrai nomes e `L-NNN`; compara com `.ref`; verifica referências `LAPIDES.md#lap-N`, presença dos nomes da pista em ambos os arquivos, SHA dos blocos de `PISTA.md` e referências às âncoras. `bin/tests/test_claude_md_regras_preservadas.sh:43–44`, `:55–77`, `:83` | Não falta nome nem âncora; os blocos da pista continuam intactos. O `.ref` só adiciona `PENDENTE-SAI` e `PRE-VERIFICADOR`, em `e25a0940:bin/tests/claude_md_regras.ref:76`, `:85`. Resultado da reprodução em memória [não verificado por execução do selo]. |

O último selo **não compara o significado dos textos consolidados**: sua verificação de `CLAUDE.md` é de nomes e referências; a comparação literal por SHA incide sobre `PISTA.md`. Portanto, permanecer verde não demonstra preservação semântica da seção 7b. `bin/tests/test_claude_md_regras_preservadas.sh:57`, `:69–74`.

Não contam como leitores de estrutura/texto: `bin/raia_agente.sh:29` e `bin/tests/test_raia_declarada_no_estado.sh:17` apenas verificam a existência do arquivo; `bin/disjuntor.py:772`, `:887–888` reconhece o **nome do anexo**, sem ler o conteúdo do documento.

## 31/10 13:21 · sessao a3feea6f45d5 · - · - · origem=Ronald (tmux)

Mapeamento da cadeia tocada, sem execução de testes: resultados de runtime e de frota são [não verificado]. [nao verificado]

| Campo/entidade | Escritor/porta | Leitor na cadeia |
|---|---|---|
| `CelulaDia.trabalha` | Regeneração calcula o valor pelo vínculo e cria/escreve na porta; fora da vigência escreve `False`. Reversão restaura pelo DNA; geração em período cria e atualiza futuro/presente. — `app/ponto/portas/celula.py:231`, `:243`, `:247`, `:260`, `:532`, `:590`, `:593`, `:612` | Juiz lê diretamente; fábrica consulta também para distinguir limbo no gate de exportada; projetor da grade lê como fallback de tipo e previsto. — `app/chamados/juizes.py:408`; `app/chamados/services/fabrica_da_celula.py:153`; `app/escala/services/leitor_celula.py:344`, `:353` |
| `ata['tipo_dia']` | Grade produz folga/trabalho e overlays alteram para folga/falta/ausência; cartório copia à ata; porta persiste a ata. — `app/escala/utils.py:727`, `:752`, `:1118`, `:1174`, `:1180`, `:1743`, `:1749`; `app/ponto/services/cartorio.py:567`; `app/ponto/portas/celula.py:730`, `:741`, `:768`, `:775` | Juiz; projetor da grade; `_marcos_faltantes` repete veto de tipo diferente de trabalho. — `app/chamados/juizes.py:408`; `app/escala/services/leitor_celula.py:344`; `app/chamados/services/declaracao_texto.py:90`, `:95` |
| `ata['sem_ancora']` | Fonte 1: `work is None`; fonte 2: `_sem_anc_i` no intermitente; fonte 3: `eh_dia_trabalho(...) is None` no não intermitente. Cartório copia o booleano e a mesma porta persiste a ata. — `app/escala/utils.py:729`, `:1514`, `:1701`, `:1752`; `app/ponto/services/cartorio.py:570`; `app/ponto/portas/celula.py:768` | Projetor copia para grade; folha usa como causa e fechamento conta dias incertos. Cluster e intervalo deixam de ler a chave; juiz só lê `trabalha`/`tipo_dia`. — `app/escala/services/leitor_celula.py:362`; `app/folha/causas.py:96`; `app/ponto/services/fechamento.py:1018`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`; `app/chamados/juizes.py:407` |
| `PerguntaDisputa` | Fábrica chama núcleo de disputa e geração de perguntas; geração chama `PerguntaDisputa.abrir`; gate reaponta existente ou cria. — `app/chamados/services/fabrica_da_celula.py:124`; `app/chamados/services/disputa_emissao.py:598`, `:606`, `:618`, `:389`; `app/chamados/models.py:2408`, `:2451`, `:2453` | Fábrica lê identidade do fato e carimbo; estado consulta juiz `pergunta_viva`; resposta grava pela função central; materialização lê e pode carimbar paridade. — `app/chamados/services/fabrica_da_celula.py:103`; `app/chamados/models.py:2476`; `app/chamados/services/disputa_emissao.py:1212`, `:1266`; `app/chamados/services/materializacao.py:818`, `:843`, `:860`, `:870` |
| `ChamadoColaborador` | Núcleo de disputa chama `abrir` para chamado pai; pergunta recém-nascida órfã chama reconciliador; cluster abre pela mesma porta e fecha por `reconciliar`. Gate trava colaborador antes do dedup e cria com `get_or_create`. — `app/chamados/services/disputa_emissao.py:504`, `:404`; `app/ponto/services/cluster_espurio.py:83`, `:88`; `app/chamados/models.py:864`, `:865`, `:930` | Cluster lê vivos para fechar e todos estados para dedup; intervalo lê vivos e reconcilia legado. — `app/ponto/services/cluster_espurio.py:45`, `:58`; `app/ponto/management/commands/detectar_intervalo_ausente.py:123`, `:138` |

A operação de editar célula não consta entre as operações registradas da porta; o registro relaciona gerar/regenerar/reverter, lavrar e carimbar. O delta de produção dos quatro emissores troca decisões/leitores e acrescenta escrita de Pauta pela porta `escrever`, sem acrescentar escrita direta de célula/pergunta/chamado nesses emissores. — `app/ponto/portas/celula.py:66`; `app/chamados/services/fabrica_da_celula.py:80`, `:153`, `:185`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`.

`_plantio_preenche_furo_na_data` é leitor, com decisão utilizada tanto no preview quanto no ato de materializar: lê célula do dia, chama juiz, monta slots e marcas, e devolve o resultado do propositor; o preview usa na guarda de paridade e o ato usa antes de recusar ou plantar. — `app/chamados/services/materializacao.py:156`, `:160`, `:164`, `:167`, `:438`, `:818`.

Defeitos adicionais:

- **Bypass da Pauta no fio mudo:** o segundo universo de `disparar_perguntas_competencia` inclui chamados vivos com célula sem filtrar `trabalha=True`; calcula motivos, exclui pergunta já existente e chama `fabrica.perguntar` diretamente, sem passar pelo gate de competência/Pauta. O encadeamento de produção está configurado com `--comp-corrente --apply`. Portanto, sob a condição de competência corrente entregue + limbo com chamado vivo + fato ainda sem pergunta, a rota pode criar a pergunta que o julgamento desviou para Pauta; ocorrência real na frota [não verificado]. — `app/chamados/management/commands/disparar_perguntas_competencia.py:155`, `:167`, `:220`, `:276`, `:288`, `:295`; `app/config/crons.py:1464`; `app/chamados/services/fabrica_da_celula.py:153`.
- **O gate não cumpre o literal amplo “pergunta nova só em aberta”:** trabalha `True` exportado continua perguntando, pois apenas `None` consulta a entrega; há teste que exige esse comportamento. — `app/chamados/services/fabrica_da_celula.py:153`, `:158`; `app/chamados/tests/test_f1c4_3_emissores.py:207`.
- **Intervalo não é emissor de nascimento nesta cópia:** detector alterado alimenta reconciliação de chamados legados; o nascimento foi aposentado e o comando incrementa suprimidos e faz `continue`. Os testes dessa função provam detecção de furos, não criação de chamado. — `app/ponto/management/commands/detectar_intervalo_ausente.py:119`, `:123`, `:138`, `:149`, `:155`; `app/chamados/tests/test_f1c4_3_emissores.py:139`.
- **Sem conclusão sobre COL717:** o código genérico conserva/reabre o chamado encontrado pelo lookup, mas não há, no dado fornecido, estado concreto do primeiro chamado ou seus lookups para provar preservação nominal. — `app/chamados/models.py:865`, `:880`, `:883`; [não verificado].

## 31/10 13:20 · sessao 8ee4ed027e0c · - · - · origem=Ronald (tmux)

São 23 testes: 18 de emissores e 5 do desvio para Pauta; a análise abaixo prevê a morte dos mutantes por inspeção, sem execução [não verificado]. As asserções estão em `app/chamados/tests/test_f1c4_3_emissores.py:80-153` e `:199-239`.

Abreviações: `T` = `app/chamados/tests/test_f1c4_3_emissores.py`; `F` = `app/chamados/services/fabrica_da_celula.py`; `M` = `app/chamados/services/materializacao.py`; `C` = `app/ponto/services/cluster_espurio.py`; `I` = `app/ponto/management/commands/detectar_intervalo_ausente.py`; `J` = `app/chamados/juizes.py`.

| Teste — nome após `test_` | Mutação mínima que prevê vermelho | [nao verificado]
|---|---|
| `RED_fabrica_limbo_pergunta` | Restaurar `cel.trabalha is not True` no veto de `F:80`; desaparece a lista exigida em `T:80-81`. |
| `fabrica_folga_nao_pergunta` | Trocar o retorno do veto por `['orfao_14h']`, em `F:81`; viola `T:83-84`. **Remover apenas o juiz não mata este teste**, pois há outro veto em `app/chamados/services/declaracao_texto.py:95-96`. |
| `MORDE_fabrica_dia_de_trabalho_pergunta` | Veto incondicional em `F:80`, retornando `[]`; viola `T:86-87`. |
| `fabrica_ausencia_lavrada_com_trabalha_true_nao_pergunta` | Trocar o retorno do veto por `['orfao_14h']`, em `F:81`; viola `T:89-91`. Remover apenas a cláusula `tipo_dia` do juiz **sobrevive**, pelo segundo veto de `declaracao_texto.py:95-96`. |
| `RED_materializacao_limbo_furo_absolve_o_plantio` | Restaurar `cel.trabalha is not True` em `M:160`; devolve `False` em `M:169`, violando `T:100-101`. |
| `materializacao_folga_nao_absolve` | Remover a chamada ao juiz do veto de `M:160`, deixando `if not lamp`; a fixture alcança o plantio de `M:167-168`, contrariando `T:103-104`. |
| `MORDE_materializacao_dia_de_trabalho_absolve` | Veto incondicional em `M:160`; viola `T:106-107`. |
| `RED_materializacao_ausencia_lavrada_com_trabalha_true_nao_absolve` | Em `M:160`, substituir juiz por `cel.trabalha is False`; a ausência passa ao plantio e viola `T:109-110`. |
| `RED_cluster_limbo_conta` | Restaurar `cel.trabalha is not True` em `C:28`; viola `T:118-119`. |
| `cluster_folga_nao_conta` | Em `C:28`, usar só `cel.trabalha is False`; a fixture `None/folga` chega ao `append` de `C:35`, violando `T:121-122`. |
| `MORDE_cluster_dia_de_trabalho_conta` | Veto incondicional em `C:28`; viola `T:124-125`. |
| `RED_cluster_ausencia_lavrada_com_trabalha_true_nao_conta` | Em `C:28`, usar só `cel.trabalha is False`; viola `T:127-128`. |
| `cluster_sem_ancora_com_trabalha_true_conta` | Acrescentar `or ata.get('sem_ancora')` em `C:28`; viola `T:130-131`. |
| `RED_intervalo_limbo_conta` | Restaurar `cel.trabalha is not True` em `I:51`; viola `T:140-141`. |
| `intervalo_folga_nao_conta` | Em `I:51`, usar só `cel.trabalha is False`; viola `T:143-144`. |
| `MORDE_intervalo_dia_de_trabalho_conta` | Veto incondicional em `I:51`; viola `T:146-147`. |
| `RED_intervalo_ausencia_lavrada_com_trabalha_true_nao_conta` | Em `I:51`, usar só `cel.trabalha is False`; viola `T:149-150`. |
| `intervalo_sem_ancora_com_trabalha_true_conta` | Acrescentar `or ata.get('sem_ancora')` em `I:51`; viola `T:152-153`. |
| `RED_limbo_em_exportada_nao_pergunta_e_vai_a_pauta_dp` | Desativar o bloco de `F:153`, deixando seguir para `perguntar` em `F:160`; viola retorno/perguntas/Pauta de `T:199-205`. |
| `limbo_em_aberta_pergunta_sem_pauta` | Negar o resultado de `competencia_do_dia_entregue` em `F:155`; aberta vai para Pauta e retorna `[]`, violando `T:207-210`. |
| `MORDE_trabalho_em_exportada_segue_perguntando_sem_pauta` | Retirar `and cel.trabalha is None` de `F:153`; passa a desviar também `True`, violando `T:212-216`. |
| `RED_limbo_em_exportada_duas_vezes_uma_pauta` | Remover `ja.update(...)` de `F:182`; segunda passagem escreve outra Pauta, violando `T:218-225`. |
| `RED_limbo_em_exportada_pauta_feita_nao_reabre` | Acrescentar `feito_em__isnull=True` ao filtro de `F:180`; a feita some da dedup e nasce outra, violando `T:227-239`. |

As mutações dos dois negativos da fábrica que trocam `[]` por lista mostram apenas que os testes observam o resultado; uma mutação relevante de arquitetura — retirar o juiz — sobrevive porque `_marcos_faltantes` também cala `tipo_dia != 'trabalho'`. São necessários dois desvios para liberar esse caso pelo fluxo normal: retirar o veto em `F:80` e o veto em `app/chamados/services/declaracao_texto.py:95-96`; o encadeamento ocorre em `F:49` e `app/chamados/services/disputa_emissao.py:826-827`.

Não há indicação de fixture vazia nos positivos: a fábrica e o plantio recebem entrada acesa e saída apagada; cluster recebe duas órfãs a dois minutos de distância; intervalo recebe entrada/saída final acesas e duas pausas apagadas (`T:45-49`, `:77`, `:96`, `:115`, `:136-137`). Os positivos exigem lista, `True`, dia ou marcos concretos, e os testes aberta/trabalho exportada contam efetivamente `PerguntaDisputa` (`T:81`, `:101`, `:119`, `:141`, `:209`, `:215`). [nao verificado]

Há ramos e contratos sem prova:

- Os testes dos quatro emissores chamam helpers; só a segunda classe chama `perguntar_pela_celula`. Não demonstram o ato de materializar, abrir chamado de cluster ou executar o command de intervalo (`T:75-78`, `:94-98`, `:113-116`, `:134-138`, `:184-187`; atos de cluster em `C:87-105` e command em `I:102-119`). [nao verificado]
- A fixture escreve `CelulaDia` por `create`/`update`, sem chamar a lavra; portanto o teste não prova o formato produzido pelo cartório nem a interação com a ata em curso (`T:65-72`; projeção do julgamento em `J:392-395`). [nao verificado]
- Nenhum caso fornece `trabalha=False`, `tipo_dia` ausente/vazio ou pergunta anterior carimbada; os valores construídos e casos são os de `T:65-153`, e o ramo de desfazer carimbo fica em `F:145-148`.
- Nenhum teste cria Pauta cancelada, resposta filha ou contexto longo; os únicos históricos exercitados são viva repetida e feita (`T:218-239`). A consulta nova exige cabeça `pai__isnull=True` em `F:180`. [nao verificado]
- O caso feita chama a porta, mas não verifica `feito_em`; tornar `marcar_feita_pelo_sistema` um no-op mantém seus asserts e o cenário fica equivalente ao teste de viva. Atualmente a porta grava `feito_em` em `app/pautas/services.py:261-266`; o teste só confere retorno, quantidade e perguntas (`T:233-239`).
- O caso de exportada verifica apenas que existe algum `[motivo:`, sem exigir o motivo exato nem o dia no texto; a busca só usa âncora/DP/prefixo (`T:189-205`).
- O teste `MORDE_trabalho_em_exportada...` exige uma pergunta nova em exportada, apesar do aval literal do usuário “Pergunta NOVA ... só em competência ABERTA” [não verificado quanto ao aval em arquivo]; a exigência está em `T:212-216` e a produção a implementa em `F:153-160`. [nao verificado]

Outro defeito concreto por construção: mover as tags ao início não garante idempotência contra truncamento. `escrever` reserva `len(_ctx)` sem teto e corta o corpo em `500-len(_ctx)-4`; o contexto inclui nome e todas as batidas, sem limite nessa montagem (`app/pautas/services.py:168-177`, `:413`, `:419-420`). Se o espaço restante cortar o marcador ou uma tag, o próximo julgamento não encontra `startswith` ou `[motivo:...]` e escreve outra Pauta (`F:180-190`). Ocorrência desse caso em produção: [não verificado].

## 31/10 13:20 · sessao 6818fb16157f · - · - · origem=Ronald (tmux)

Achados para consolidação (referências do snapshot atual):

- **Disparo sem persistência pode desaparecer na troca de sessão.** `_com_estado` mantém o retorno da decisão quando `os.replace` falha; não reconstitui o estado pela trilha. A próxima sessão só varre seus próprios subagentes. Portanto: Stop dispara → gravação falha → sessão muda → novo final da obra passa, sem linha nova. (`bin/disjuntor.py:263`, `bin/disjuntor.py:269`, `bin/disjuntor.py:319`, `bin/disjuntor.py:670`, `bin/disjuntor.py:1096`)
- **Disparo persistido também pode ser contornado por exceção.** Um transcript de verificador ilegível, inclusive de outra obra, aborta `varrer_b3` antes da consulta decisória ao estado; `main_pretool` libera a ferramenta. Trava ocupada e estado ilegível chegam ao mesmo caminho. É comportamento herdado do mecanismo de falha aberta, mas impede a garantia absoluta do aval. (`bin/disjuntor.py:326`, `bin/disjuntor.py:534`, `bin/disjuntor.py:673`, `bin/disjuntor.py:247`, `bin/disjuntor.py:254`, `bin/disjuntor.py:1132`)
- **Fotografia falha permite promover linha antiga.** `_topo_shas` troca leitura falha por `[]`; edição posterior de qualquer parte do RELATO atualiza o mtime. A linha antiga, ausente da fotografia, passa nas duas verificações sem ter sido escrita depois do disparo. (`bin/disjuntor.py:638`, `bin/disjuntor.py:643`, `bin/disjuntor.py:607`, `bin/disjuntor.py:609`)
- **Readback não fecha a corrida com redisparo.** A confere consumo persistido e `disparado=False`; B dispara novamente depois dessa conferência; A escreve LIBERA e retorna sucesso. O novo final passa com a obra novamente disparada e sem linha posterior ao segundo disparo. Confirmei a transição em memória, injetando o segundo disparo durante a chamada de trilha LIBERA. (`bin/disjuntor.py:622`, `bin/disjuntor.py:624`, `bin/disjuntor.py:631`, `bin/disjuntor.py:634`, `bin/disjuntor.py:699`, `bin/disjuntor.py:1096`)
- **Concorrência também causa negativa indevida.** A recebe obra disparada; B consome a linha e persiste a liberação; A entra em `libera_b3`, encontra `disparado=False`, recebe `None` e nega mesmo assim. (`bin/disjuntor.py:604`, `bin/disjuntor.py:612`, `bin/disjuntor.py:1096`, `bin/hook_stop_fila1.py:1045`)
- **Stop decide com candidato anterior à fotografia.** O Stop lê topo+mtime antes de varrer; `varrer_b3` fotografa novamente sob a trava. Escrita/remoção entre as leituras permite que `libera_b3` receba texto obsoleto, distinto do texto fotografado. O mtime foi lido separadamente e antes do conteúdo. (`bin/hook_stop_fila1.py:1044`, `bin/hook_stop_fila1.py:1045`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:700`, `bin/disjuntor.py:738`, `bin/disjuntor.py:741`)
- **Linha válida posterior pode não liberar por identidade textual.** Reescrever depois do disparo a mesma linha já consumida continua proibido; o hash representa conteúdo, não ocorrência de escrita. O selo exige expressamente essa recusa. (`bin/disjuntor.py:555`, `bin/disjuntor.py:607`, `bin/tests/test_disjuntor.sh:376`, `bin/tests/test_disjuntor.sh:379`)
- **Migração muda a referência temporal de disparo antigo.** Estado antigo já disparado recebe `disparado_em=agora` e fotografia atual; uma linha válida escrita depois do disparo original, mas antes da migração, não libera. Confirmei esse caminho em memória. (`bin/disjuntor.py:655`, `bin/disjuntor.py:657`, `bin/disjuntor.py:607`, `bin/disjuntor.py:609`)

Todos os campos são persistidos pela porta `_com_estado`, com flock e substituição do JSON; a leitura genérica é `ler_estado`. (`bin/disjuntor.py:229`, `bin/disjuntor.py:242`, `bin/disjuntor.py:263`, `bin/disjuntor.py:275`)

| Campo | Escritores | Leitores |
|---|---|---|
| `reprovados` | inicializa/append: `bin/disjuntor.py:690`, `:694`; limpa: `:612`, `:662`, `:719` | `bin/disjuntor.py:611`, `:658`, `:661`, `:693`, `:697`, `:717`, `:718` |
| `rearmado` | migração `bin/disjuntor.py:661`; rearme `:718` | `bin/disjuntor.py:661`, `:691`, `:718` |
| `liberado` | consumo `bin/disjuntor.py:611` | `bin/disjuntor.py:611`, `:691` |
| `disparado` | inicialização `bin/disjuntor.py:690`; dispara `:699`; limpa `:612`, `:719` | `bin/disjuntor.py:604`, `:624`, `:655`, `:698`, `:705`, `:745` |
| `disparado_em` | `bin/disjuntor.py:657`, `:699` | `bin/disjuntor.py:609`, `:656` |
| `consumidas` | `bin/disjuntor.py:613` | `bin/disjuntor.py:607`, `:613`, `:623` |
| `topo_no_disparo` | `bin/disjuntor.py:657`, `:700` | `bin/disjuntor.py:607` |
| `b3_limiar` | `bin/disjuntor.py:663` | `bin/disjuntor.py:651`, `:660` |

| Trilha | Escritor | Leitor de produção |
|---|---|---|
| `DISJUNTOR B3` | varredura `bin/disjuntor.py:701` → `dispara`, `:133` → `trilha`, `:120` | `ler`, `bin/disjuntor.py:1180`, `:1202`; apresentação `:1295` |
| `LIBERA` | `bin/disjuntor.py:634` | `bin/disjuntor.py:1180`, `:1198`; devolve `b3_libera`, `:1238`; apresenta `:1308` |
| `MIGRA` | `bin/disjuntor.py:659` | Não entra na lista lida por `ler`; o selo procura a linha. (`bin/disjuntor.py:1180`, `bin/tests/test_disjuntor.sh:330`) |
| `FALHA` | RELATO `bin/disjuntor.py:224`; persistência `:269`; consumo `:626`, `:629`, `:632`; B4 `:1110`; PreToolUse `:1133`; Stop `bin/hook_stop_fila1.py:1056` | `ler` lê todas, mas somente `relato_negacao` incrementa o painel; as demais não reconstroem estado. (`bin/disjuntor.py:1180`, `:1196`, `:1197`) |

## 31/10 13:20 · sessao 232c2fc7e8f8 · - · - · origem=Ronald (tmux)

Conclusões da integração, sem modificar arquivos:

- **Corrida que aceita linha anterior ao disparo real.** Stop captura `agora`, depois captura topo/mtime, só então dispara; a fotografia usada no disparo pode conter outro topo. Uma candidata capturada pelo Stop, retirada antes da fotografia, passa porque seu hash não está em `topo_no_disparo` e seu mtime supera o relógio capturado antes. Fontes: `bin/hook_stop_fila1.py:1038`, `bin/hook_stop_fila1.py:1044`, `bin/hook_stop_fila1.py:1045`, `bin/hook_stop_fila1.py:1046`, `bin/disjuntor.py:607`, `bin/disjuntor.py:609`, `bin/disjuntor.py:699`, `bin/disjuntor.py:700`. Sonda em memória com funções reais confirmou `disparado=False` e consumo da candidata; não há artefato gravado. [não verificado — registro persistido da execução]

- **Corrida que nega apesar da linha válida.** PreToolUse varre e observa disparo; Stop consome a linha entre essa varredura e `libera_b3` do PreToolUse; esse `libera_b3` encontra `disparado=False`, retorna `None` e o chamador nega a ferramenta. Fontes: `bin/disjuntor.py:1096`, `bin/disjuntor.py:604`, `bin/disjuntor.py:605`, `bin/disjuntor.py:1097`, `bin/hook_stop_fila1.py:1046`.

- **Outra falsa negativa temporal.** `_topo_e_mtime` lê mtime antes do conteúdo; escrita válida concorrente pode produzir conteúdo novo associado ao mtime antigo, que `libera_b3` rejeita. Fontes: `bin/disjuntor.py:738`, `bin/disjuntor.py:741`, `bin/disjuntor.py:609`.

- **Trava parcial.** Cada leitura/mutação/persistência de estado usa flock, mas `varrer_b3 → libera_b3 → resposta` não forma operação única. A conferência de consumo ocorre fora da trava, mediante `ler_estado`. Fontes: `bin/disjuntor.py:229`, `bin/disjuntor.py:242`, `bin/disjuntor.py:257`, `bin/disjuntor.py:272`, `bin/disjuntor.py:616`, `bin/disjuntor.py:622`, `bin/disjuntor.py:1096`.

Checklist:

| Classe | Conclusão |
|---|---|
| Segundo juiz registrado | Não demonstrado: trata-se de instrumento host; o reconhecedor duplicado anterior foi removido, com lápide. `bin/disjuntor.py:4`; `bin/hook_stop_fila1.py:411`. |
| Segundo escritor fora da porta | Não demonstrado: mutações B3 passam por `_com_estado`; linha automática no RELATO usa `no_relato`. `bin/disjuntor.py:616`; `bin/disjuntor.py:708`; `bin/disjuntor.py:721`; `bin/disjuntor.py:221`. |
| Relógio fora da borda | Callers de produção passam `agora`; auxiliares mantêm fallback `time.time()`. A corrida principal é instante capturado antes da fotografia. `bin/hook_stop_fila1.py:1038`; `bin/hook_stop_fila1.py:1045`; `bin/disjuntor.py:1096`; `bin/disjuntor.py:653`; `bin/disjuntor.py:669`. |
| Derivação paralela | Reconhecedor e leitura de topo foram centralizados; não encontrei duplicação nessa integração. `bin/disjuntor.py:560`; `bin/disjuntor.py:598`; `bin/disjuntor.py:733`; `bin/hook_stop_fila1.py:1044`; `bin/hook_stop_fila1.py:1046`. |
| Ordem de trava | Cai nas corridas acima; chamadas separadas não ficam serializadas como decisão completa. `bin/disjuntor.py:616`; `bin/disjuntor.py:622`; `bin/disjuntor.py:1096`. |
| Pouso antes de pré-requisito | CLAUDE exige regra/ponteiro e tem uma linha; o RELATO distingue construção de pouso depois do produto. O commit isolado não prova deploy/pouso da obra. `CLAUDE.md:502`; `app/docs/RELATO.md:61`; `app/docs/BACKLOG.md:779`. |

A frase “para a obra / antes de nova volta” é mais ampla que a trava implementada: ferramentas comuns, construtor e chamadas Codex seguem fora do B3; só novo `Agent` verificador reconhecido como final recebe a trava. Entretanto, isso já está explicitamente limitado no contrato técnico do RELATO. Fontes: `bin/disjuntor.py:1075`, `bin/disjuntor.py:1084`, `bin/disjuntor.py:1093`, `bin/disjuntor.py:1096`, `app/docs/RELATO.md:11`, `app/docs/RELATO.md:13`.

O Stop pode sair antes de varrer B3 por orçamento, PAREI, espera comprovada, fila vazia ou teto; isso é limitação expressamente documentada, e PreToolUse varre novamente antes da decisão. Fontes: `bin/hook_stop_fila1.py:1213`, `bin/hook_stop_fila1.py:1221`, `bin/hook_stop_fila1.py:1227`, `bin/hook_stop_fila1.py:1241`, `bin/hook_stop_fila1.py:1288`, `bin/hook_stop_fila1.py:1295`, `app/docs/RELATO.md:13`, `bin/disjuntor.py:1096`.

## 31/10 13:19 · sessao 4a3e8aa54332 · - · - · origem=Ronald (tmux)

O `_C18` tem uma autoridade registrada: `chamados/juizes.py::dia_sem_trabalho`; `core/juizes.py` declara a pergunta e o endereço, sem outra implementação concorrente nessa pergunta. O juiz de vínculo responde previsão, e o de status responde status lavrado, perguntas distintas. (`app/core/juizes.py:142`, `app/core/juizes.py:266`, `app/core/juizes.py:165`, `app/core/juizes.py:180`)

O predicado real é `trabalha is False or bool(tipo_dia and tipo_dia != 'trabalho')`: ausência de `tipo_dia` não cala, `None` não cala e `sem_ancora` não participa. `_celula_furada` chama esse juiz, sem duplicar o predicado. (`app/chamados/juizes.py:407`, `app/chamados/juizes.py:408`, `app/chamados/juizes.py:443`)

Há respostas próprias de produção que permanecem:

| Sítio | Resposta própria / diferença |
|---|---|
| `_marcos_faltantes` | `tipo_dia != 'trabalho'` elimina marcos de cobrança; não incorpora `trabalha=False` com ata contraditória e elimina tipo ausente/indefinido. (`app/chamados/services/declaracao_texto.py:90`, `app/chamados/services/declaracao_texto.py:95`) |
| `classificar_dia` | Declara `COBRANCA_EM_DIA_SEM_TRABALHO` usando tipo diferente de trabalho, fora de `SEM_JURISDICAO`, previsto zero, chamados vivos e ausência de cobertura; predicado diferente e mais restrito que C18. (`app/ponto/supra_juiz.py:443`, `app/ponto/supra_juiz.py:445`) |
| `celula_veta_cobranca` | `trabalha is False` vira `FOLGA`; ignora cláusula da ata, além de vetos próprios de veredito. (`app/ponto/marcos.py:155`, `app/ponto/marcos.py:159`, `app/ponto/marcos.py:161`) |
| Emissão de ausências | Usa o juiz paralelo `celula_veta_cobranca`. (`app/ponto/management/commands/detectar_ausencias.py:625`) |
| Emissão de reconciliação da grade | Usa o mesmo juiz paralelo. (`app/ponto/management/commands/reconciliar_grade.py:502`) |
| Emissão/reabertura c2 | Importa o mesmo juiz paralelo. (`app/chamados/reconciliador.py:768`) |
| Leitor de furos sem cobrança | Usa esse veto para rotular a porta. (`app/ponto/services/furos_sem_cobranca.py:98`) |
| Lote de perguntas, primeiro universo | Seleciona exclusivamente `trabalha=True`, eliminando limbo antes de qualquer chamada ao juiz. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`) |
| Vigia de hora | Seleciona exclusivamente `trabalha=True` antes de chamar o cartório; limbo fica fora desse caminho de emissão. (`app/ponto/services/vigia_de_hora.py:21`, `app/ponto/services/vigia_de_hora.py:130`) |

Também existem testemunhas que apresentam o limbo como não cobrável: worklist elimina todo tipo diferente de trabalho; relatório de furos elimina `sem_ancora`; frase para admin/IA usa `not cel.trabalha`; relatório diário conta `None` como folga. São leitores/relatórios, não portas diretas de nascimento de chamado. (`app/ponto/worklist.py:19`, `app/relatorios/furos.py:151`, `app/relatorios/furos.py:157`, `app/relatorios/furos.py:218`, `app/relatorios/furos.py:237`, `app/ponto/services/dia_do_colab.py:121`, `app/relatorios/dia_da_frota.py:47`)

Dois defeitos importantes para a consolidação:

1. O gate novo só protege `trabalha=None`; `True` em exportada chega a `perguntar`, contrariando o literal atual “pergunta NOVA só competência ABERTA”. O comentário da obra mantém a exceção antiga O44 como se prevalecesse. (`app/chamados/services/fabrica_da_celula.py:151`, `app/chamados/services/fabrica_da_celula.py:153`, `app/chamados/services/fabrica_da_celula.py:158`)
2. O lote chama `fabrica.perguntar` diretamente, pulando o gate novo de `perguntar_pela_celula`. Seu universo de fio mudo consulta chamados com célula sem restringir `trabalha`, busca motivo pela ata e aceita fato sem pergunta existente; portanto há caminho para pergunta nova de limbo exportado sem Pauta. (`app/chamados/management/commands/disparar_perguntas_competencia.py:155`, `app/chamados/management/commands/disparar_perguntas_competencia.py:167`, `app/chamados/management/commands/disparar_perguntas_competencia.py:220`, `app/chamados/management/commands/disparar_perguntas_competencia.py:226`, `app/chamados/management/commands/disparar_perguntas_competencia.py:277`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`)

O registro C18 não ganhou padrão específico em `PROIBIDOS['chamado']`, que cobre apenas SLA e dia cru do chamado; registrar o endereço não impede essas derivações. (`app/core/juizes.py:1368`, `app/core/juizes.py:1375`)

## 31/10 13:19 · sessao 54380a25fb11 · - · - · origem=Ronald (tmux)

Achados do selo e documentação:

- **O mutante limiar=3 fica vermelho antes de medir a primeira reprovação.** O caso `b3` exige literalmente `MIGRA ... limiar 3 -> 1` em `bin/tests/test_disjuntor.sh:330`; a constante mutada produz `-> 3` por `bin/disjuntor.py:659`. Assim, o mutante declarado em `bin/tests/test_disjuntor.sh:845` pode morrer nessa comparação antes de chegar à prova da primeira reprovação em `bin/tests/test_disjuntor.sh:339`. Há mordida textual, sem isolamento causal do limiar.

- **Mutante relevante que sobreviveria aos casos B3, por inspeção:** acrescentar `and lei != 'escritor unico'` ao predicado em `bin/disjuntor.py:576`. As aceitações de `b3_topo` usam juiz único, célula soberana e lâmpada em `bin/tests/test_disjuntor.sh:675`, `bin/tests/test_disjuntor.sh:694`, `bin/tests/test_disjuntor.sh:695`. A única linha de escritor único do caso integrado está numa sequência que espera continuar bloqueada, em `bin/tests/test_disjuntor.sh:385` e `bin/tests/test_disjuntor.sh:387`. Não executei a suíte porque cria arquivos — `bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:51`.

- **Outro sobrevivente, por inspeção:** trocar `>= B3_BOBO_MIN` por `> B3_BOBO_MIN` em `bin/disjuntor.py:581`: não há motivo com exatamente 20 caracteres nos casos explícitos; eles cobrem `typo` e motivo longo em `bin/tests/test_disjuntor.sh:696` e `bin/tests/test_disjuntor.sh:697`.

- **Divergência literal do aval:** mínimo de 20 caracteres é requisito acrescentado em `bin/disjuntor.py:75`, aplicado em `bin/disjuntor.py:581`; o texto fornecido pelo usuário pede somente uma linha de motivo [não verificado no arquivo]. Execução isolada do parser em AST, só em memória, confirmou que `ERRO-BOBO OBRA-X: faltou import` é recusado; o comportamento vem de `bin/disjuntor.py:581`. O contrato local já explicita essa restrição em `app/docs/RELATO.md:12`; portanto distinguir divergência contra aval literal de implementação contra contrato local.

- **A forma aceita não demonstra verdade semântica:** `DESENHO ... pergunta=x app/inexistente.py:0` passa pelo parser; regex aceita zero em `bin/disjuntor.py:76`, e a validação só exige uma palavra adicional em `bin/disjuntor.py:576` e `bin/disjuntor.py:577`. Não verifica arquivo existente, linha existente, lei efetivamente quebrada ou ausência de juiz.

- **CLAUDE.md atende ao recorte pedido:** uma linha com nome e ponteiros em `CLAUDE.md:502`; a referência preserva o nome em `bin/tests/claude_md_regras.ref:93`. O diff real avô→atual acrescenta apenas essa linha no CLAUDE e o nome no `.ref` — posições atuais citadas acima.

- **O próprio contrato reconhece limites relevantes:** falha aberto, saídas do Stop antes de varrer B3, e PreToolUse como trava principal estão expressos em `app/docs/RELATO.md:13`; não apresentar essas propriedades como se fossem garantias universais de bloqueio.

## 31/10 13:08 · sessao 0b3b9a70c232 · GRADE-FOLGA · verificador · origem=Code

**Há um bloqueador: o calendário adaptou o formato, mas continua fornecendo um universo de folgas incompatível com a fonte compartilhada.** Isso preserva divergências entre calendário, grade e precedência (`app/colaboradores/services/calendario.py:593`, `app/colaboradores/services/calendario.py:607`; `app/ponto/precedencia.py:207`).

1. **Quem escreve e quem lê o novo formato**

   O carregador produz listas com todas as `FolgaDia` da pessoa, ordenadas por pk; o calendário produz tuplas `(True,)` somente com datas do vínculo ativo (`app/ponto/precedencia.py:207`; `app/colaboradores/services/calendario.py:593`, `app/colaboradores/services/calendario.py:607`).

   | API/formato | Todos os chamadores diretos encontrados em produção |
   |---|---|
   | `folgas_lancadas_do_periodo` | `app/ponto/precedencia.py:282`; `app/escala/utils.py:1119` |
   | `folga_lancada_do_dia` | `app/ponto/precedencia.py:358`; `app/escala/utils.py:1124` |
   | `folga_lancada_vale_no_dia` | `app/ponto/precedencia.py:218` |
   | Argumento explícito `folgas_lancadas=` | `app/colaboradores/services/calendario.py:607` |

   Não encontrei chamadas diretas dessas APIs nem outro fornecimento explícito do parâmetro em testes [nao verificado]. Os testes novos exercitam os helpers indiretamente pela grade e por `fatos_do_dia` (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:57`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:62`).

   **Os leitores das listas estão corretos:** ambos passam as candidatas ao seletor; este percorre até encontrar uma válida. O `bool(folga_lancada)` da saída ocorre depois da seleção (`app/ponto/precedencia.py:217`, `app/ponto/precedencia.py:358`, `app/ponto/precedencia.py:408`; `app/escala/utils.py:1124`).

   **O erro é a alimentação alternativa do calendário:**

   - `folgas_do_periodo` conserva vínculo e data, mas descarta `calendario_origem_id`; o calendário carrega apenas o vínculo ativo (`app/escala/alimentacao.py:66`; `app/colaboradores/services/calendario.py:593`).
   - Quando `_cobre`, fornece `(True,)` e impede a carga completa da pessoa; `True` é aceito como folga manual incondicionalmente (`app/colaboradores/services/calendario.py:602`, `app/colaboradores/services/calendario.py:607`; `app/ponto/precedencia.py:281`, `app/ponto/precedencia.py:238`).
   - **Falso positivo:** o ativo cobre a janela, mas a célula declara outro vínculo como geradora. Uma folga de calendário do ativo passa como manual, embora devesse ser rejeitada contra o vínculo escolhido pela juíza (`app/ponto/precedencia.py:327`, `app/ponto/precedencia.py:351`, `app/ponto/precedencia.py:240`; `app/colaboradores/services/calendario.py:607`).
   - **Falso negativo:** uma folga manual presa ao vínculo antigo desaparece dessa alimentação, embora o carregador completo a encontre e o predicado a aceite (`app/colaboradores/services/calendario.py:593`; `app/ponto/precedencia.py:207`, `app/ponto/precedencia.py:238`).

   Isso muda a apresentação: `_fatos` alimenta `folga_humana`, que força `trabalho=False` e pode devolver status `folga` (`app/colaboradores/services/calendario.py:792`, `app/colaboradores/services/calendario.py:801`, `app/colaboradores/services/calendario.py:116`, `app/colaboradores/services/calendario.py:134`). A correção deve fornecer as candidatas reais pelo carregador compartilhado, ou deixar `fatos_do_periodo` carregá-las (`app/ponto/precedencia.py:281`).

2. **Duas ou mais FolgaDia: a seleção foi corrigida nos dois builders**

   Não há leitor que selecione simplesmente a primeira candidata bruta nesses caminhos: grade e precedência usam o mesmo seletor, que testa cada candidata até a primeira válida (`app/escala/utils.py:1124`; `app/ponto/precedencia.py:217`, `app/ponto/precedencia.py:358`).

   No builder por turno, o vínculo passado vem do mapa calculado pela juíza; no legacy, o vínculo do segmento também vem dela, e o recorte elimina o dia extra `d0-1` (`app/escala/utils.py:1262`, `app/escala/utils.py:1268`, `app/escala/utils.py:1794`, `app/escala/utils.py:977`, `app/escala/utils.py:999`). Portanto, o descarte do item 3 da triagem se sustenta para esse dia extra; o ramo legacy sem vínculo continua retornando antes do overlay e não foi alterado pelo diff (`app/escala/utils.py:987`).

   A ordem agora é todas as folgas, depois as ausências; o tratamento conserva folga diante de falta aprovada e permite que ausência não falta a sobreponha (`app/escala/utils.py:1794`, `app/escala/utils.py:1797`, `app/escala/utils.py:1178`). A medição desse impacto mencionada na triagem não foi apresentada [nao verificado].

3. **Versão 3 e horizonte**

   `REGRA_VERSOES` é lida pela validação no import e por `versao_para`; este escolhe a maior versão aplicável à competência, sem teto superior (`app/ponto/services/cartorio.py:177`, `app/ponto/services/cartorio.py:201`, `app/ponto/services/cartorio.py:206`).

   | Competência | Versão resultante |
   |---|---|
   | Até 2026-08 | `None` |
   | 2026-09 | `1` |
   | Desde 2026-10, inclusive futuras | `3` |

   Essas respostas seguem a comparação dos horizontes com a competência derivada da janela da empresa, não do mês civil (`app/ponto/services/cartorio.py:203`, `app/ponto/services/cartorio.py:206`; `app/ponto/janelas.py:60`).

   O chamador de produção de `versao_para` é `_regra_do_dia`, usado nos dois ramos que montam a impressão. A mudança 2→3 altera o hash e permite sair do pulo por impressão igual na próxima execução do cartório (`app/ponto/services/cartorio.py:957`, `app/ponto/services/cartorio.py:1009`, `app/ponto/services/cartorio.py:1161`, `app/ponto/services/cartorio.py:362`, `app/ponto/services/cartorio.py:1167`).

   Os selos da declaração viva foram atualizados. As expectativas restantes de versões 1/2 usam declarações substituídas por `patch` ou valores passados diretamente ao hash; o teste de `nunca_bateu` consulta a versão viva (`app/ponto/tests/test_o219_versao_da_regra.py:76`, `app/ponto/tests/test_o219_versao_da_regra.py:89`, `app/ponto/tests/test_o219_versao_da_regra.py:112`, `app/ponto/tests/test_o219_versao_da_regra.py:136`, `app/ponto/tests/test_o219_versao_da_regra.py:151`, `app/ponto/tests/test_o219_versao_da_regra.py:159`, `app/ponto/tests/test_o219_versao_da_regra.py:166`; `app/ponto/tests/test_selo_b5u_nunca_bateu_ve_ausencia.py:128`).

   **Exportação não é filtro da versão:** qualquer competência exportada desde 2026-10 seria alcançada (`app/ponto/services/cartorio.py:206`). O comentário afirma exportadas somente até 09, mas existência efetiva de exportada alcançada não está demonstrada pelos dados fornecidos [nao verificado] (`app/ponto/services/cartorio.py:172`).

4. **O item 6 da triagem precisa voltar como bloqueador**

   A parte do calendário não responde “pode lançar?”: alimenta a classificação exibida. Logo, sua adaptação não podia parar em `True → (True,)` (`app/colaboradores/services/calendario.py:607`, `app/colaboradores/services/calendario.py:792`, `app/colaboradores/services/calendario.py:799`).

   Os outros dois sítios enfileirados continuam sendo caminhos de porta e não consomem o novo formato: `o_dia_ja_e_folga` consulta o vínculo do dia; `_folga_avulsa` alimenta o veto de lançamento (`app/ponto/services/declarar_dia.py:90`; `app/ponto/precedencia.py:165`, `app/ponto/precedencia.py:501`). Isso não absolve suas regras, mas não demonstra uma regressão de formato causada por este diff nesses dois sítios (`app/ponto/services/declarar_dia.py:70`; `app/ponto/precedencia.py:166`).

5. **Classes de reprovação**

   - **Derivação paralela de fonte única: ocorre no calendário**, pela reconstrução incompleta das candidatas já descrita (`app/colaboradores/services/calendario.py:607`; `app/ponto/precedencia.py:207`).
   - **Segundo juiz novo:** o predicado foi extraído da precedência e compartilhado; não identifiquei duplicação nova da autoridade registrada nesses trechos (`app/core/juizes.py:160`; `app/ponto/precedencia.py:223`, `app/ponto/precedencia.py:358`; `app/escala/utils.py:1124`).
   - **Segundo escritor persistente, relógio fora da borda ou ordem nova de travas:** os trechos acrescentados carregam dados e alteram dicionários locais; não acrescentam escrita ORM, leitura de relógio ou aquisição de trava (`app/ponto/precedencia.py:207`; `app/escala/utils.py:1118`, `app/escala/utils.py:1125`).
   - **Cada teste novo fica vermelho sem a cura? Não.** Por inspeção, quatro das dez execuções herdadas são controles que já passavam: calendário velho no legacy, manual velha por turno e calendário novo nos dois builders. O próprio teste reconhece os dois primeiros; coexistência e manual fora da janela fornecem contraexemplos para ambos (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:21`, `:80`, `:85`, `:91`, `:97`, `:102`). Execução efetiva RED/GREEN [nao verificado].
   - **Pouso antes do pré-requisito:** o ensaio `--apply` é exigido expressamente, mas seus resultados e eventual pouso não foram fornecidos [nao verificado] (`app/ponto/services/cartorio.py:173`, `app/ponto/services/cartorio.py:175`).

## 31/10 13:06 · sessao 5d7fa2e39603 · - · - · origem=Ronald (tmux)

Não encontrei defeito novo nos dois builders quanto à seleção de múltiplas candidatas: o seletor percorre cada candidata até achar a primeira válida; grade e precedência usam esse mesmo seletor (`app/ponto/precedencia.py:217`, `app/ponto/precedencia.py:358`, `app/escala/utils.py:1124`).

- **Legacy / descarte da triagem 3:** o vínculo de cada segmento vem da juíza; `lambda _d: esc` corresponde ao vínculo dos dias preservados. O dia extra `d0-1` é removido pelo recorte (`app/escala/utils.py:972`, `app/escala/utils.py:977`, `app/escala/utils.py:770`, `app/escala/utils.py:995`, `app/escala/utils.py:999`).
- **Legacy sem vínculo:** retorna diretamente dias como folga e pula o overlay; não chama `montar_grade_prevista(None)`. Isso já existia e não é alterado pelo diff fornecido (`app/escala/utils.py:987`, `app/escala/utils.py:989`, `app/escala/utils.py:991`).
- **Por turno:** o mapa passado ao overlay é calculado pela mesma juíza; não há escolha paralela do vínculo acrescentada nesta obra (`app/escala/utils.py:1262`, `app/escala/utils.py:1268`, `app/escala/utils.py:1794`).
- **Ordem dos overlays:** agora todas as folgas precedem todas as ausências, como triagem 2 declara. Ausência não falta vence folga; falta aprovada conserva folga. A ordem está expressa no código; a medição na sombra citada pela triagem não veio como resultados auditáveis (`app/escala/utils.py:1794`, `app/escala/utils.py:1797`, `app/escala/utils.py:1178`) — medição [nao verificado].
- **RED não vale para cada teste individual:** o próprio arquivo reconhece que calendário velho já passava no legacy e mão velha já passava por turno. O caso calendário do vínculo novo também é controle que já passava pelos caminhos anteriores do diff fornecido (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:21`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:80`). Assim, por inspeção, **4/10 execuções herdadas são controles verdes antes da cura**; isso não torna o selo inteiro vazio: coexistência e mão fora da janela têm contraexemplos nos dois builders (`app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:91`). Execução RED/GREEN/suite [nao verificado].
- **Juiz duplicado:** a seleção compartilhada extrai o predicado anterior da precedência e ambas as testemunhas o leem; não identifiquei segundo juiz novo nesses trechos (`app/ponto/precedencia.py:223`, `app/ponto/precedencia.py:358`, `app/escala/utils.py:1115`). A autoridade registrada para precedência permanece `fatos_do_dia` (`app/core/juizes.py:160`).
- **Escrita/relógio/travas:** os novos caminhos carregam FolgaDia e alteram somente o dicionário local da grade; não acrescentam escrita ORM, leitura de relógio ou aquisição de trava (`app/ponto/precedencia.py:207`, `app/escala/utils.py:1118`, `app/escala/utils.py:1125`).

_1051 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
