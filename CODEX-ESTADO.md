# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 15:18** · publicado em **05/11/2026 15:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 15:18 · sessao d02a48d17352 · - · - · origem=Ronald (tmux)

Censo servidor, somente leitura:

- **Porta normal:** `/api/` inclui `api.urls`, que aponta `ponto/bater/` para `api.views.api_bater_ponto` (`app/config/urls.py:37`; `app/api/urls.py:2,67`). **Porta kernel:** `config.urls_core` inclui `api.urls_core`, que importa `views_core` e aponta a mesma rota para sua função (`app/config/urls_core.py:6`; `app/api/urls_core.py:5,14`). Qual configuração atende produção: **[nao verificado]**.
- **Identidade:** ambos exigem autenticação e obtêm `colab = request.user.colaborador`; não escolhem colaborador a partir do corpo (`app/api/views.py:398-404`; `app/api/views_core.py:385-391`). A configuração admite JWT e sessão (`app/config/settings/base.py:32-43`). Portanto, sob sessão válida de B, batida guardada quando A estava logado será atribuída a B, se passar os gates — inferência estática, certeza alta (`app/api/views.py:402,683-695`).
- **CSRF:** sessão e middleware CSRF estão configurados (`app/config/settings/base.py:39,56-59`); a fila manda `X-CSRFToken` obtido do cookie atual (`app/static/pwa.js:140,153-157`). A função não tem `csrf_exempt`, apenas decorators DRF (`app/api/views.py:398-400`). Validação efetiva pelo framework instalado: **[nao verificado]**; o contrato de SessionAuthentication é inferido da classe configurada (`app/config/settings/base.py:39`).
- **Gates relevantes:** tipo precisa ser E/S; latitude/longitude são obrigatórios; falta de GPS dá 403 (`app/api/views.py:432-439,453-466`; kernel: `app/api/views_core.py:419-426,440-458`). Situação fora de ativo/afastado dá 403 (`app/api/views.py:415-423`). Assim, atribuição a B é um caminho possível, não garantia de aceitação (`app/api/views.py:402,415-423,453-466`).

**`timestamp_dispositivo`: escritores e leitores**

1. Cliente grava ISO bruto na fila (`app/static/pwa.js:113-119`) e o envia como campo multipart (`app/static/pwa.js:147`). O `timestamp` corrigido pelo gap é usado para ordenar a fila, mas o multipart não o envia; também não envia `timestamp_gap_ms`, `gps_accuracy` ou `gps_captured_at` (`app/static/pwa.js:137,143-157`).
2. APIs leem o bruto para auditoria de GPS ausente e payload de assinatura (`app/api/views.py:459,497-504`; `app/api/views_core.py:451,489-496`), e para decidir a hora efetiva por `hora_do_aparelho` (`app/api/views.py:536-539`; `app/api/views_core.py:528-531`).
3. `hora_lida` aceita epoch/ISO, normaliza timezone ou devolve None (`app/api/hora_do_aparelho.py:98-121`). `hora_do_aparelho` usa o timestamp do aparelho se estiver entre agora e 48h atrás; fora disso usa agora, preservando o timestamp legível bruto (`app/api/hora_do_aparelho.py:62-67,135-153`).
4. As APIs passam `timestamp_dispositivo=ts_aparelho` ao escritor `registrar_batida` (`app/api/views.py:682-695`; `app/api/views_core.py:679-692`). Ele encaminha `**campos` a `Batida.objects.create`; campo do modelo é nullable (`app/ponto/registro_batida.py:94,152-159`; `app/ponto/models.py:75`).
5. Leitor persistido: template DP mostra a coluna “dispositivo” (`app/templates/ponto/batidas_offline_pendentes.html:70-75`), porém a view alimentadora seleciona **somente** `origem='offline_pendente', aprovada=False` (`app/ponto/views.py:441-446`), enquanto o endpoint atual grava `origem='app'` (`app/api/views.py:683-684`). Logo, existência desse leitor não prova que novas batidas da fila apareçam nessa tela — certeza alta (`app/ponto/views.py:441-446`; `app/api/views.py:684`).
6. Outro leitor persistido: `descartadas` compara `timestamp` e `timestamp_dispositivo` acima da tolerância de 0,05h (`app/api/hora_do_aparelho.py:67,156-185`). Chamador dessa função fora de testes não foi localizado; execução em produção **[nao verificado]**.
7. `ponto.views.bater_ponto` recebe o formulário web legado, usa colaborador da sessão e delega POST E/S para `ramo_registrar_es`; não é a URL usada pela fila (`app/ponto/views.py:241-249,268-272,346-354`; `app/static/pwa.js:153`). `ponto.views.batida_offline` está aposentada e responde 410, apesar do decorator `csrf_exempt` (`app/ponto/views.py:417-428`). Não confundir esse `csrf_exempt` com a API atual (`app/api/views.py:398-400`).
8. Há leituras semelhantes em **ping geográfico**, não no recebimento da batida da fila: `app/api/views.py:2343-2361`; `app/api/views_core.py:881-899`; `app/ponto/views.py:2929,2987`.

**Deduplicação: transmissão repetida não implica duas linhas**

- API devolve `ja_registrada` quando encontra mesmo colaborador + mesmo tipo em janela inclusiva ±5min do **timestamp efetivo**, retornando sucesso (`app/api/views.py:557-575`; kernel: `app/api/views_core.py:549-567`). Esse filtro não exclui batidas retratadas (`app/api/views.py:559-562`).
- Chokepoint retorna batida existente do mesmo tipo, não retratada, em ±60s (`app/ponto/registro_batida.py:106-115`); depois verifica a chave exata colaborador+timestamp, inclusive outro tipo/retratada (`app/ponto/registro_batida.py:116-122`).
- Tipo oposto tem envelope de 300s com escala/marcos, senão 60s; pode ser permitido quando casa com marco próprio; sem autoridade humana, colisão é recusada (`app/ponto/registro_batida.py:21-49,138-151`). A API transforma essa recusa em resposta de sucesso `ignorada_intervalo` (`app/api/views.py:698-721`).
- Modelo declara unicidade por **colaborador+timestamp**, sem tipo e sem ID do evento/fila (`app/ponto/models.py:102-106`). Verificação prévia e create são separados; concorrência com mesmo timestamp pode chegar à constraint, sem tratamento local nesse helper (`app/ponto/registro_batida.py:120-122,157-161`). Constraint realmente aplicada ao banco de produção: **[nao verificado]**.

**Contraexemplo concreto à idempotência universal — inferência estática, certeza alta no caminho, ocorrência [nao verificado]:** item tem timestamp bruto com mais de 48h. Primeiro envio às 10:00 usa `agora=10:00`, grava; resposta/JSON/delete falha e item permanece. Retry às 10:06 usa `agora=10:06`, pois o bruto continua antigo demais. Não encontra a primeira no filtro ±5min, no ±60s ou pela chave exata; sem outras colisões/gates, pode criar segunda batida (`app/api/hora_do_aparelho.py:141-153`; `app/static/pwa.js:158-179`; `app/api/views.py:557-562,683-695`; `app/ponto/registro_batida.py:106-122,157-161`; `app/ponto/models.py:106`).

**Contraexemplo ainda na fronteira:** primeira tentativa com bruto de 47h59min é aceita na hora antiga; resposta perdida; retry após cruzar 48h muda a hora efetiva para agora e pode escapar da deduplicação, mesmo sendo o mesmo item (`app/api/hora_do_aparelho.py:141-153`; `app/api/views.py:557-562`). Condições restantes de aceitação e ocorrência em produção: **[nao verificado]**.

## 05/11 15:18 · sessao abe6c0def59b · - · - · origem=Ronald (tmux)

**Certeza alta: conclusões estáticas abaixo; ocorrência em aparelho/produção [não verificado].**

- **Rota efetiva:** `/ponto/bater/` chama `bater_ponto` com `ui_v2=False`; `/ponto/bater-v2/` passa `ui_v2=True` (`app/ponto/urls.py:11-12`). A view seleciona `bater_ponto_v2.html` somente com `ui_v2`; caso contrário usa `_template_triagem`, cujo ramo apto escolhe `triagem/apto.html` (`app/ponto/views.py:235,356-358`). Não atribuir automaticamente a recarga do template v2 à URL `/ponto/bater/`.

- **Escritores/chamadores da fila:** `bater_ponto_v2.html` grava após exceção de `fetch` online (`app/templates/ponto/bater_ponto_v2.html:349-356`) ou se `navigator.onLine` falso (`:367-374`). Outros chamadores existentes: template antigo `app/templates/ponto/bater_ponto.html:543,597`; triagem apta `app/templates/ponto/triagem/apto.html:289`. Os chamadores fornecem tipo, coordenadas, precisão, instante GPS e foto; a gravação real é `pwa.js` (`app/static/pwa.js:107-119`). Uso em produção do template antigo [não verificado].

- **Online v2:** o formulário tem CSRF, tipo sugerido, latitude, longitude e foto (`app/templates/ponto/bater_ponto_v2.html:102-107`); o JS adiciona precisão e selfie e envia multipart para `/api/ponto/bater/`, lendo o CSRF atual do cookie (`:328-338`). **Não acrescenta `timestamp_dispositivo` no envio online v2**; esse campo nasce na gravação offline (`app/static/pwa.js:115`), e o sincronizador o envia (`:147`). `timestamp`, `timestamp_gap_ms`, `gps_accuracy`, `gps_captured_at` guardados na fila não aparecem no FormData do sincronizador (`app/static/pwa.js:143-156`).

- **Contraexemplo online+sync:** servidor aceita o POST online de `v2Confirmar`, mas a conexão cai antes de a resposta chegar; `fetch` rejeita, o catch salva uma segunda representação da mesma batida; a próxima sincronização a envia novamente (`app/templates/ponto/bater_ponto_v2.html:335-356`; `app/static/pwa.js:138-157`). Isto prova caminho para **dois envios**, não duas linhas persistidas: dedup do servidor precisa de análise separada. Cenário condicional inferido do código; ocorrência [não verificado].

- **Recarga automática v2:** `_vistoEm` começa em `Date.now()`, é sobrescrito quando a página deixa de estar visível; ao voltar visível, recarrega somente se diferença **maior** que 300000ms (`app/templates/ponto/bater_ponto_v2.html:305-310`). Não existe timer que recarregue enquanto a página continua oculta; não existe guarda de `_enviando`, foto ou operação de banco nesse callback (`:305-310`). Outra recarga ocorre em `pageshow.persisted` (`:306`), repetida globalmente no PWA (`app/static/pwa.js:430-431`).
  - **Contraexemplo:** A tira selfie, minimiza por 5min1s e volta offline: `location.reload()` é chamado; a selfie que só está em `_fotoBlob` se perde com a execução anterior; qualquer sucesso do novo carregamento depende do SW/rede (`app/templates/ponto/bater_ponto_v2.html:146,252,305-310`). Se a fila já confirmou gravação, ela está em IndexedDB e não apenas nessa variável (`app/static/pwa.js:110-122`). Resultado real [não verificado].

- **Dados nos bytes do HTML v2:** CSRF renderizado (`app/templates/ponto/bater_ponto_v2.html:103`); tipo sugerido (`:104`); posto com latitude, longitude e raio (`:139-140`); no sucesso, hora e aviso do dia (`:53-56`). A base renderiza `user.username` para autenticado (`app/templates/base.html:112,316`), e inclui painel com `data-dono="{{ user.pk }}"` (`app/templates/base.html:432`; `app/templates/core/_copiloto.html:17`). O v2 só esconde outros elementos com `display:none`, não remove seus bytes/DOM (`app/templates/ponto/bater_ponto_v2.html:148`).
  - **Nome:** nome completo não é interpolado diretamente no v2; a base contém **username**, que pode ou não coincidir com nome real [não verificado] (`app/templates/base.html:316`).
  - **Foto:** selfie é obtida em runtime por câmera/arquivo e canvas; vídeo e inputs nascem sem imagem pessoal no HTML (`app/templates/ponto/bater_ponto_v2.html:88-91,235-267`).
  - **Geolocalização:** coordenadas do **posto** são do HTML (`:139-140`); coordenadas atuais da pessoa são de `getCurrentPosition`, colocadas em variáveis e campos inicialmente vazios (`:105-106,175-180,183-195`).
  - **Contraexemplo de casca cacheada, condicional:** guardar resposta HTML de A e servi-la offline a B na mesma URL expõe username/ID, token renderizado, posto e estado de tela de A; o cache armazena resposta HTTP, não a selfie ou o GPS que o JS inseriu posteriormente. A existência desse HTML no cache depende dos ramos do SW e não está demonstrada por mera navegação (`app/static/sw.js:45-50,67-77`). Certeza alta sobre conteúdo; vazamento ocorrido [não verificado].

- **Storage adicional:** a fila do PWA usa IndexedDB `juliani-offline` v2, não localStorage (`app/static/pwa.js:48-60,107-119`). Na base há `sessionStorage` de scroll, chave `scrollpos:`+pathname, com gravação no submit e leitura/remoção no load (`app/templates/base.html:405-417`). Convite de notificações usa `sessionStorage['avisos_convite']` (`app/static/pwa.js:353,362,366`); triagem apta usa `sessionStorage['gps_aviso_visto']` (`app/templates/ponto/triagem/apto.html:278-286`). LocalStorage encontrado no script global de tabela guarda preferências sob namespace `hx.table.v2`, não batidas (`app/static/js/hasner-table.js:11,45-50`).

- **Persistência:** `navigator.storage.persisted()`/`persist()` são condicionais; ausência da API não interrompe a execução e exceções são engolidas (`app/static/pwa.js:436-443`). Ausência de API não produz garantia alternativa de durabilidade; retenção real no Safari [não verificado]. No SW existe listener `sync-batidas` (`app/static/sw.js:112-132`), mas não encontrei registro `sync.register` ou `periodicSync` nos arquivos auditados `sw.js`, `pwa.js`, `bater_ponto_v2.html`; a sincronização de página roda pelo evento online e no boot online (`app/static/pwa.js:320,323-328`). Não é necessário alegar compatibilidade de versão específica do Safari para essa conclusão.

- **Disparadores concorrentes:** `online` e boot podem chamar `sincronizarBatidas`; o listener de mensagem do SW também chama (`app/static/pwa.js:15-17,320,328`). A função não tem mutex: faz `getAll`, envia e só depois apaga (`app/static/pwa.js:126-165`). **Contraexemplo:** duas chamadas obtêm a mesma pendência antes da primeira exclusão e ambas fazem POST; igualmente duas abas têm a mesma fila e podem receber a mensagem porque o SW faz broadcast a todos (`app/static/sw.js:116-119`). Certeza alta do caminho; ocorrência e persistência duplicada [não verificado].

## 05/11 15:18 · sessao 1677a706abf6 · - · - · origem=Ronald (tmux)

O patch fornecido altera somente docstrings de `julgar` e `contador`; nenhum escritor executável, leitor ou teste muda nele (`app/ponto/services/lastro.py:219`, `app/ponto/services/lastro.py:370`).

Escritores e leitores dos valores documentados:

| Valor | Escritor | Leitores |
|---|---|---|
| `com_lastro`, `retido_pelo_fechamento` como listas | `julgar` inicializa e classifica conforme `reconciliaria()` (`app/ponto/services/lastro.py:249`, `app/ponto/services/lastro.py:314`) | `fechar` percorre ambos (`app/ponto/services/lastro.py:334`); `contador` calcula comprimentos (`app/ponto/services/lastro.py:377`); comando imprime listas e contagens (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:53`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:56`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:62`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:71`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:75`). |
| `n` do contador | `len(com_lastro)`, retido separado (`app/ponto/services/lastro.py:378`) | Chamadas encontradas em testes: `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:203`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:219`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:271`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:275`; leitor externo ou dinâmico adicional [não verificado]. |
| `_lastro`, `_retido`, `_n_lastro`, `_n_retido` | Placar chama o comando uma vez, sem `--apply`, e raspa `(a)` e `(a2)`; ausência conta 1 em cada parcela (`bin/placar_code.sh:114`, `bin/placar_code.sh:116`, `bin/placar_code.sh:118`, `bin/placar_code.sh:121`) | Publicação das duas linhas (`bin/placar_code.sh:176`, `bin/placar_code.sh:177`) e soma no `TOTAL` (`bin/placar_code.sh:549`). |
| `TOTAL` | Inclui `_n_lastro + _n_retido` (`bin/placar_code.sh:549`) | Decide publicação e saída 0 ou 2 (`bin/placar_code.sh:551`, `bin/placar_code.sh:556`, `bin/placar_code.sh:561`). |

A autoridade usada na classificação realmente coincide com a do fechamento: `reconciliaria()` chama `_decisao_do_fechamento(registrar=False)` (`app/chamados/models.py:623`), enquanto `reconciliar()` chama a mesma função com `registrar=True` e grava o estado somente quando permitida (`app/chamados/models.py:607`, `app/chamados/models.py:617`, `app/chamados/models.py:620`). Essa decisão consulta motor, guarda da premissa e guarda da pergunta viva (`app/chamados/models.py:638`, `app/chamados/models.py:648`, `app/chamados/models.py:651`).

O retido passa pela tentativa real quando `aplicar=True`; a nota só nasce se o fechamento ocorrer (`app/ponto/services/lastro.py:329`, `app/ponto/services/lastro.py:334`, `app/ponto/services/lastro.py:345`). A recusa tem escritor central em `_registrar_recusa`, deduplicado pelo payload e pelas mudanças de estado (`app/chamados/models.py:380`); os dois gates o usam (`app/chamados/models.py:408`, `app/chamados/models.py:730`).

No item 4, não há teste no diff fornecido [não verificado quanto ao commit integral, indisponível via Git na cópia]. Os testes existentes verificam comportamento, sem ler as docstrings alteradas: retenção e `n` separado (`app/ponto/tests/test_bug114_fechador_de_dois_dias.py:201`), trilha única da recusa em duas passadas (`app/ponto/tests/test_bug114_fechador_de_dois_dias.py:228`), e soma no placar para `(a)=0/(a2)=3`, `(a)=2/(a2)=0` e saída vazia (`bin/tests/test_placar_conta_o_retido_do_lastro.sh:645`). Portanto, restaurar somente os textos do patch preserva esses comportamentos (`app/ponto/services/lastro.py:314`, `app/ponto/services/lastro.py:334`, `app/ponto/services/lastro.py:378`); não caracteriza teste ineficaz de uma cura executável neste diff [não verificado quanto a execução dos testes].

O teste shell não foi executado: ele cria sentinela temporária, incompatível com só leitura (`bin/tests/test_placar_conta_o_retido_do_lastro.sh:594` — confirmar linha exata da criação, não conferida nesta exploração).

## 05/11 15:18 · sessao 4e27f6b90cf1 · MODO-COMUM-BITEMPORAL · censo · origem=Code

Os gates de desligamento indicados no cartório são **INTENCIONAIS para cobrança agora**; eles não determinam se havia trabalho em D. Já a isenção atual entra no julgamento histórico, e a demissão preenchida exclui competências anteriores sem comparar a janela: são problemas distintos. (`app/ponto/services/cartorio.py:1651`, `app/ponto/services/cartorio.py:1697`, `app/ponto/services/cartorio.py:1762`, `app/ponto/services/cartorio.py:1229`, `app/folha/export.py:840`.)

Este é um censo estático da cópia. Ocorrência, quantidade e valores afetados em produção: **[nao verificado]**. Os contraexemplos abaixo são construídos, não casos encontrados no dado raspado **[nao verificado]**. Completude absoluta de “TODO sítio”, incluindo caminhos indiretos não identificados: **[nao verificado]**.

O cartório correspondente aos pontos fornecidos está em `app/ponto/services/cartorio.py:1697` e `app/ponto/services/cartorio.py:1762`. No modelo, a situação do colaborador é `situacao`; o flag do vínculo é `EscalaColaborador.ativa`. (`app/colaboradores/models.py:323`, `app/escala/alimentacao.py:196`.)

Os contraexemplos usados na tabela final são:

| Caso | Cadastro em D → cadastro hoje → resposta histórica errada |
|---|---|
| **C1 — isenção** | Em 15/10, não isento, com furo ou 8h de HE; isenção ligada em 01/11. A consulta de outubro passa a completar marcos, eliminar retenção ou zerar eventos. A flag é editável e o leitor central não recebe data. (`app/colaboradores/views.py:176`, `app/core/constants.py:13`, `app/escala/utils.py:904`, `app/folha/export.py:407`.) |
| **C2 — vínculo** | Em D, vínculo A diurno; hoje A encerrado e B noturno ativo desde 01/11. O leitor escolhe B para interpretar D, embora exista autoridade para escolher A pela célula/vigência. (`app/ponto/turnos.py:1556`, `app/escala/alimentacao.py:228`.) |
| **C3 — template** | Célula de D congelada em 08–17; template editado para 09–18. O builder casa os marcos antigos, mas alinha somente às colunas novas: missing antigos deixam de ocupar coluna e batidas casadas podem virar órfãs. Variante: DNA 19–07, template atual 07–19; a saída esperada muda de D+1 para D. (`app/escala/utils.py:1390`, `app/escala/utils.py:1628`, `app/escala/utils.py:1729`, `app/escala/utils.py:1740`, `app/escala/servico_jornada.py:208`.) |
| **C4 — intervalo** | D tinha pausa fixa de 60min; hoje o mesmo template declara 90min ou modo livre. O mínimo/desconto de D passa a seguir o cadastro novo. Para o realizado, 08–17 com pausa declarada e sem batidas de intervalo pode deixar de descontar a pausa quando o modo atual vira livre. (`app/ponto/juiz_batida.py:270`, `app/ponto/juiz_batida.py:299`, `app/ponto/turnos.py:758`, `app/ponto/turnos.py:788`.) |
| **C5 — situação e pertencimento** | Em outubro, ativo na empresa A/posto P; hoje desligado ou transferido para B/Q. O relatório de outubro omite a pessoa ou atribui seu histórico a B/Q. Se A entregou a competência e B não, também muda a resposta sobre entrega/tranca. Esses estados podem efetivamente ser escritos pelo código. (`app/colaboradores/services/desligamento.py:104`, `app/colaboradores/views.py:1385`, `app/colaboradores/views.py:1386`, `app/chamados/juizes.py:938`.) |
| **C6 — demissão** | Competência encerrada em 20/10, trabalhada normalmente; demissão em 30/10. Exportar outubro hoje exclui a pessoa como rescisão porque testa apenas `data_demissao != None`. O corte transcrito manda excluir demissão **na janela**. (`app/folha/export.py:834`, `app/folha/export.py:840`.) |
| **C6b — reativação** | Houve desligamento e depois reativação. A reativação limpa `data_demissao`; consultas antigas passam a dispor apenas da situação ativa e do último par de datas, sem representar a interrupção anterior. (`app/colaboradores/services/desligamento.py:210`, `app/colaboradores/services/desligamento.py:211`, `app/holerite/matriz.py:87`, `app/ponto/services/fechamento.py:430`.) |
| **C7 — CCT** | Em D, empresa A/CCT, aplicação ativa e parâmetro de dobra ligado; hoje empresa B/CLT, aplicação desativada ou parâmetro alterado. A mesma competência recebe outra régua. A vigência da CCT é comparada com D, mas aplicação, empresa e valores vêm dos registros atuais. (`app/core/regua_cct.py:254`, `app/core/regua_cct.py:261`, `app/core/regua_cct.py:352`, `app/core/regua_cct.py:374`.) |
| **C8 — corte e janela HE** | Corte da empresa era 21; hoje é 26: a competência 10 muda de 21/09–20/10 para 26/09–25/10. Na HE, D já estava após `janela_he_desde`, com piso zero; hoje piso 15: chegada 12min antes passa a receber outro tratamento. A data inicial protege dias anteriores, mas não versiona alterações posteriores dos parâmetros. (`app/ponto/janelas.py:50`, `app/ponto/janelas.py:57`, `app/ponto/motor_calculo_v2.py:1392`, `app/ponto/motor_calculo_v2.py:1649`.) |
| **C9 — régua/diagnóstico** | D pertencia a vínculo confiável de 4h; hoje há vínculo/template de 8h ou `regua_classe=LIMBO`. O passado correto vira desvio, cadastro suspeito ou furo descartado. (`app/escala/services/meio_periodo.py:152`, `app/escala/services/meio_periodo.py:174`, `app/escala/services/cadastro_realidade.py:202`, `app/escala/services/escala_certa.py:32`.) |
| **C10 — tipo de ausência** | Ausência de D usava tipo abonado; hoje o tipo vira descontável ou inativo. Variantes: `fim` passa de indeterminado a definido; `parcial` ou `medico` é desligado. A classificação/cobertura histórica muda porque `vigente()` significa catálogo de agora. O próprio cadastro promete preservar histórico ao desativar. (`app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:267`, `app/ponto/catalogo/ausencias.py:293`, `app/ponto/models.py:607`.) |
| **C11 — município** | Em D, o posto estava no município X, com feriado municipal; hoje o mesmo posto aponta para Y. Mesmo escolhendo o vínculo de D, o lugar vem do `Posto.municipio` atual e D pode perder o feriado. (`app/core/feriados.py:121`, `app/colaboradores/services/estrutura.py:64`.) |
| **C12 — unidade da rubrica** | TXT antigo contém bruto 800, originalmente 8h; o cadastro atual do código passa a indicar dias. O PDF histórico interpreta o mesmo bruto como 800 dias. O número vem do TXT, mas sua unidade vem do cadastro atual. (`app/folha/pdf_export.py:34`, `app/folha/pdf_export.py:96`, `app/folha/pdf_export.py:117`.) |

Os juízes existentes não cobrem igualmente essas perguntas. `vinculo_do_dia` resolve o vínculo; `EscalaColaborador.marcos_do_dia` lê o DNA; isso não transforma os atributos atuais do objeto retornado em versões históricas. (`app/docs/JUIZES.md:22`, `app/docs/JUIZES.md:98`, `app/escala/alimentacao.py:279`, `app/escala/models.py:1124`.)

Na tabela final, **V** significa `vinculo_do_dia`; **M**, `EscalaColaborador.marcos_do_dia`/`marcos_turno`; **A**, `ausencia_cobre`/`cobertura_ausencia_periodo`; **J**, `janela_fechamento`/`janela_atual`; **R**, `regua_para`/`_aplicacao_vigente`; **F**, `classificar_export`/`prontidao`. Todos constam do registro. (`app/docs/JUIZES.md:22`, `app/docs/JUIZES.md:30`, `app/docs/JUIZES.md:35`, `app/docs/JUIZES.md:47`, `app/docs/JUIZES.md:74`, `app/docs/JUIZES.md:98`.)

**“Sem histórico registrado”** significa que o juiz atual existe ou o campo existe, mas uma autoridade que responda sua versão vigente em D não aparece nessa tabela; existência em outro registro: **[nao verificado]**. O plano adia versões de vínculo, escala e regra com `vigente_de/ate` e `registrado_em`; essa ordem de obra não autoriza substituir D pelo cadastro atual. (`app/docs/JUIZES.md:20`, `app/docs/PLANO-PADROES.md:13`, `app/docs/CORTES.md:288`.)

Quem escreve e quem lê os campos citados:

| Campo mutável | Quem escreve | Quem lê |
|---|---|---|
| `Colaborador.isento_ponto` | Passe Livre, com trilha, pela porta `gravar`. (`app/colaboradores/views.py:176`, `app/colaboradores/views.py:185`, `app/colaboradores/services/colaborador.py:55`.) | Juiz atual `isento_ponto`; cartório, grade, espelho, fechamento, exportação e detectores enumerados abaixo. (`app/core/constants.py:18`, `app/ponto/services/cartorio.py:1141`, `app/folha/export.py:407`.) |
| `situacao` | Desligamento, reativação, lançamento de afastamento e reversor. (`app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:210`, `app/ponto/views.py:2425`, `app/ponto/management/commands/reverter_situacao_afastado.py:73`.) | Gates do cartório, universos de relatórios, fechamento e comandos abaixo; cadastro corrente também filtra ativo. (`app/ponto/services/cartorio.py:1697`, `app/relatorios/dia_da_frota.py:34`, `app/ponto/services/fechamento.py:430`, `app/colaboradores/queries.py:26`.) |
| `data_demissao` | Agendamento/desligamento; reativação limpa. (`app/colaboradores/services/desligamento.py:86`, `app/colaboradores/services/desligamento.py:105`, `app/colaboradores/services/desligamento.py:211`.) | Exportação, recorte do espelho, universo do fechamento, leitor da célula e matriz do holerite. (`app/folha/export.py:840`, `app/folha/export.py:1061`, `app/ponto/services/fechamento.py:423`, `app/escala/services/leitor_celula.py:351`, `app/holerite/matriz.py:88`.) |
| `data_admissao` | Edição cadastral e salvamento inteiro pela porta. (`app/colaboradores/services/colaborador.py:34`, `app/colaboradores/services/colaborador.py:73`.) | Recorte de retenção, leitor da célula e matriz do holerite. (`app/folha/export.py:1180`, `app/escala/services/leitor_celula.py:351`, `app/holerite/matriz.py:88`.) |
| `Colaborador.empresa/posto` | Edição cadastral e transferência; o salvamento inteiro também permite persistir esses campos. (`app/colaboradores/views.py:1313`, `app/colaboradores/views.py:1342`, `app/colaboradores/views.py:1385`, `app/colaboradores/views.py:1388`, `app/colaboradores/services/colaborador.py:73`.) | Régua, fechamento, relatórios, ranking e competência/entrega dos chamados abaixo. (`app/core/regua_cct.py:352`, `app/ponto/services/fechamento.py:435`, `app/folha/services/ranking_he.py:122`, `app/chamados/juizes.py:938`.) |
| Vínculo: `ativa`, `tipo_escala`, `posto`, início/fim e âncora | Criação/alteração, fechamento de vigência e correção pela porta de vínculo. (`app/colaboradores/services/vinculo.py:265`, `app/colaboradores/services/vinculo.py:278`, `app/colaboradores/services/vinculo.py:561`, `app/colaboradores/services/vinculo.py:797`, `app/colaboradores/services/vinculo.py:922`.) | V e seus carregadores; leitores que ainda escolhem o ativo estão na tabela final. (`app/escala/alimentacao.py:148`, `app/escala/alimentacao.py:228`, `app/ponto/turnos.py:1556`.) |
| TipoEscala: horários, pausas, modo, duração, ciclo e overrides | Cadastro do tipo; o admin do tipo também permanece editável. (`app/escala/services/cadastro_tipo.py:217`, `app/escala/services/cadastro_tipo.py:229`, `app/escala/services/cadastro_tipo.py:245`, `app/escala/admin.py:8`.) | Grade, serviço de jornada, juiz de batida, motor e calculadores abaixo. (`app/escala/utils.py:1390`, `app/escala/servico_jornada.py:71`, `app/ponto/juiz_batida.py:125`, `app/ponto/motor_calculo_v2.py:832`.) |
| `regua_classe` e campos da avaliação | Avaliação, corroboração/desfazimento e porta `gravar_regua`. (`app/escala/regua_defesa.py:667`, `app/escala/regua_defesa.py:722`, `app/escala/regua_defesa.py:766`, `app/colaboradores/services/vinculo.py:969`.) | Vetos, apuração de furos e confiabilidade da escala em D. (`app/escala/regua_defesa.py:644`, `app/escala/services/furos_diarios.py:36`, `app/escala/services/escala_certa.py:32`.) |
| `Empresa.dia_inicio_competencia/regime_trabalhista` | Formulário e porta da estrutura; admin mantém editáveis os demais campos da empresa, bloqueando especificamente o regime. (`app/colaboradores/views.py:523`, `app/colaboradores/views.py:538`, `app/colaboradores/services/estrutura.py:17`, `app/colaboradores/admin.py:19`.) | J e R; os consumidores herdam a janela/régua atual. (`app/ponto/janelas.py:50`, `app/core/regua_cct.py:353`.) |
| `Empresa.janela_he_*`, `limite_decisao_he_min` | Cadastro de Empresa, inclusive admin; esses campos não estão no bloqueio específico do regime. (`app/colaboradores/models.py:58`, `app/colaboradores/models.py:82`, `app/colaboradores/admin.py:19`.) | Motor/espelho e atos de recusa/revogação. (`app/ponto/motor_calculo_v2.py:1390`, `app/ponto/services/espelho.py:387`, `app/ponto/management/commands/lavrar_he_pendente.py:54`.) |
| `AplicacaoConvencao.ativo` | Porta de cadastrar/desativar. (`app/core/services/aplicacao_convencao.py:130`, `app/core/services/aplicacao_convencao.py:151`.) | `_aplicacao_vigente`. (`app/core/regua_cct.py:254`.) |
| Sindicato: vigência e parâmetros CCT | Formulário sobrescreve o registro, com trilha. (`app/core/views_config.py:307`, `app/core/views_config.py:310`, `app/core/views_config.py:320`.) | R compara a vigência e lê os valores atuais. (`app/core/regua_cct.py:261`, `app/core/regua_cct.py:374`.) |
| `ParametroSistema.regua_cct_ativa` | Escritor ativo dessa chave na cópia: **[nao verificado]**; a documentação informa que ela não está na tela de parâmetros. (`app/core/views_config.py:443`.) | `get_motor_cct`. (`app/core/regua_cct.py:444`.) |
| TipoAusencia: `ativo/efeito/fim/parcial/medico/bloqueia_ponto` | Tela → `gravar_tipo_ausencia`, sobrescrevendo o tipo. (`app/ponto/views_tipos_ausencia.py:67`, `app/ponto/views_tipos_ausencia.py:86`, `app/ponto/services/ausencia.py:1285`.) | Catálogo atual e seus consumidores de folha, cobertura, status e relatórios abaixo. (`app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:267`.) |
| `Posto.municipio` | Atualização do posto e backfill; ambos acionam reavaliação. (`app/colaboradores/services/estrutura.py:64`, `app/colaboradores/services/estrutura.py:86`.) | Lugar do vínculo e alcance do feriado. (`app/core/feriados.py:121`, `app/core/feriados.py:151`.) |
| Rubrica: código/tipo/ativo | `vincular_evento` atualiza; `desvincular_evento` remove o vínculo. (`app/folha/rubricas.py:27`, `app/folha/rubricas.py:38`.) | Mapeamento do export e interpretação do PDF histórico. (`app/folha/export.py:962`, `app/folha/pdf_export.py:96`.) |

Os selos existentes deixam passar a classe pelos seguintes limites:

- A catraca semselo5 aceita **HOJE** ou **ABERTO**. Os sítios históricos já declarados continuam permitidos; ela procura principalmente `EscalaColaborador.ativa`, e declara formas não cobertas. Não cobre, por esse significado, empresa, isenção ou versões internas de regras. (`app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:5`, `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:11`, `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:21`, `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:216`.)
- O contrato do juiz de escala caça tomadas da geradora e cópias da pergunta de vínculo. Isso não equivale a provar que os atributos do vínculo/template retornado sejam históricos. O próprio selo declara ausência de padrões proibidos gerais nessa família. (`app/escala/tests/test_contract_juiz_escala.py:28`, `app/escala/tests/test_contract_juiz_escala.py:124`.)
- O teste de isenção do export **exige** zerar eventos de snapshot descrito como anterior à isenção. Ele cristaliza a leitura atual; não apresenta duas vigências da flag. (`app/folha/tests/test_isento_cargo_export.py:30`, `app/folha/tests/test_isento_cargo_export.py:58`.)
- O selo E3 da isenção prova mudança de impressão e horizonte. Não prova início/fim da isenção por colaborador; o horizonte de regra é outra dimensão temporal. (`app/ponto/tests/test_e3_isento_na_impressao.py:78`, `app/ponto/tests/test_e3_isento_na_impressao.py:195`, `app/ponto/tests/test_e3_isento_na_impressao.py:230`.)
- O selo de régua pelo vínculo do dia muda vínculo/praça/ciclo mantendo a empresa; não exercita transferência, desativação da aplicação ou alteração posterior dos parâmetros da CCT. (`app/core/tests/test_semselo5_regua_pelo_vinculo_do_dia.py:47`.)
- Os contratos do export cobram que os leitores obedeçam ao juiz, inclusive sua trava de âncora. Concordar com o mesmo juiz atual não prova que ele tenha escolhido o cadastro de D. (`app/folha/tests/test_contrato_export_um_juiz.py:154`, `app/folha/tests/test_txt_le_a_celula_e_o_juiz.py:244`.)
- O selo dos crons lista sete arquivos e procura escolha de vínculo por `ativa=True`; a isenção atual no início do detector continua fora dessa pergunta. (`app/ponto/tests/test_c3_crons_perguntam_a_juiza.py:17`, `app/ponto/tests/test_c3_crons_perguntam_a_juiza.py:44`, `app/ponto/management/commands/processar_alertas_avancados.py:32`.)
- Os testes de dia da frota criam pessoas ativas e verificam contagem/geradora; não fazem a transição posterior de situação/empresa do exemplo C5. (`app/relatorios/tests/test_dia_da_frota.py:30`, `app/relatorios/tests/test_dia_da_frota.py:46`, `app/relatorios/tests/test_dia_da_frota.py:78`.)
- O contrato do catálogo exige ler `vigente()`, definido como catálogo de agora. O selo de configuração exige declarar consumidor/efeito, sem exigir vigência dos valores consumidos. (`app/ponto/tests/test_contract_catalogo_congelado.py:7`, `app/ponto/tests/test_contract_catalogo_congelado.py:24`, `app/core/tests/test_contract_configuracao_nao_mente.py:93`.)
- Os testes da janela HE protegem D anterior a `janela_he_desde`; não demonstram versões sucessivas do piso/teto dentro da vigência já iniciada. (`app/ponto/tests/test_janela_he_regra.py:107`, `app/ponto/tests/test_janela_he_saida.py:376`.)

Há exclusões importantes para não transformar toda leitura atual em BUG: o calendário já alimenta a autoridade com geradoras e verifica cobertura antes de usar a foto; cabeçalhos de cadastro atual não demonstram, sozinhos, uma decisão histórica; e o DIFF que reproduz o comportamento antigo é instrumento de impacto. (`app/colaboradores/services/calendario.py:505`, `app/colaboradores/services/calendario.py:510`, `app/folha/views_retrato.py:89`, `app/relatorios/management/commands/diff_extrato_parcial.py:2`.)

O corte de quarentena por período exige a data da flag e distingue cobrança de cálculo; ele não constitui um juiz histórico de isenção da folha. L-124 limita o alcance das obras e mantém a regra da competência, sem fornecer vigência individual da flag. (`app/docs/CORTES.md:348`, `app/docs/CORTES.md:352`, `app/docs/LEIS.md:145`.)

Na tabela, BUG significa divergência estática no contrafactual indicado. Execução desses contrafactuais e incidência no dado de produção: **[nao verificado]**.

| arquivo:linha | campo | decide | INTENCIONAL/BUG | juiz que devia ler |
|---|---|---|---|---|
| `app/ponto/services/cartorio.py:1697` | `situacao` atual | Emitir agora cobrança de furo de D | **INTENCIONAL** — HX-CARTORIO-UNIVERSO: julga todos, emite para quem responde (`:1651`) | — |
| `app/ponto/services/cartorio.py:1762` | `situacao` atual | Emitir agora cobrança parcial de D | **INTENCIONAL** — mesmo corte em `:1651` | — |
| `app/ponto/services/cartorio.py:1298`, `:1711`, `:1763` | Afastamento agora | Suspender emissão sobre D | **INTENCIONAL** — corte 19/10, explicitamente “AGORA”, em `:1281` e `:1286` | `afastado_na_batida` já lido |
| `app/ponto/services/cartorio.py:1141`, `:1229`, `:1419` | `isento_ponto` atual | Julgar cada célula histórica | **BUG C1** | Sem histórico registrado da isenção |
| `app/ponto/services/cartorio.py:890`, `:1246`, `:1806` | `isento_ponto` atual | Status lavrado de D | **BUG C1** | Status registrado existe; insumo histórico da isenção não |
| `app/escala/utils.py:904`, `:909` | `isento_ponto` atual | Completar marcos de todos os dias da grade | **BUG C1** | Sem histórico registrado da isenção |
| `app/escala/services/leitor_celula.py:349`, `:412` | `isento_ponto` atual | Sobrepor isenção à ata de D | **BUG C1** | Sem histórico registrado da isenção |
| `app/escala/services/leitor_celula.py:351` | Últimas admissão/demissão | Recorte dos dias lidos | **BUG C6b** quando houve reativação; simples recorte pela data efetiva é intencional | Sem histórico registrado do contrato empregatício |
| `app/folha/export.py:407` | `isento_ponto` atual | Eventos da co

[truncado em 20000 de 39486 caracteres]

## 05/11 15:18 · sessao 0807d2a2c0fb · REVISAO-CRON-15 · verificador · origem=Code

**Há achado no item 4:** o selo declara provar causa única, mas continua verde quando outro teste marcado ainda falha por sobreposição (`app/core/tests/test_marca_cron15.py:67-71`; `app/chamados/tests/test_contract_crons.py:348-367`).

1. **Quem escreve e quem lê.**

   | Campo/valor acrescentado | Escritor | Leitor |
   |---|---|---|
   | `PRAZO` | `app/core/tests/marca_cron15.py:24` | `no_prazo`, no mesmo arquivo:54 |
   | `ITEM_DE_FILA` | `app/core/tests/marca_cron15.py:25` | `app/core/tests/test_marca_cron15.py:89` |
   | `CORREDOR_S` | `app/core/tests/marca_cron15.py:26` | Duração fabricada em `app/core/tests/test_marca_cron15.py:69` |
   | `P95_QUE_CABE_S` | `app/core/tests/marca_cron15.py:27` | Leitor efetivo: `[nao verificado]` |
   | `MARCADOS` | `app/core/tests/marca_cron15.py:29-47` | Contagem/comparação em `app/core/tests/test_marca_cron15.py:49-50`; importação dos objetos:62-65 |
   | `_HOJE_REAL` | Importação em `app/core/tests/marca_cron15.py:49`; mocks em `app/core/tests/test_marca_cron15.py:79,84` | `app/core/tests/marca_cron15.py:54` |
   | `__marca_cron15__` | `app/core/tests/marca_cron15.py:59` | `app/core/tests/test_marca_cron15.py:65` |
   | Falha esperada | Delegada a `unittest.expectedFailure` em `app/core/tests/marca_cron15.py:62` | Flag conferida em `app/core/tests/test_marca_cron15.py:80,85` |

   O decorator consulta o prazo, mas não consulta `MARCADOS`; a restrição aos nomes depende da comparação por AST (`app/core/tests/marca_cron15.py:57-62`; `app/core/tests/test_marca_cron15.py:25-50`).

   As 15 aplicações estão em `app/chamados/tests/test_contract_crons.py:49,57,102,107,118,138,145,173,185,314,397`, `app/chamados/tests/test_cron_longo_sai_da_medida.py:53,64,125` e `app/core/tests/test_contract_crons_duracao_horario.py:52`.

   Fora do diff, o medidor escreve as durações majoradas no JSON; a configuração carrega esse JSON em `DUR_MAX_S`; `inicio_derivado` lê a duração para produzir ou recusar o horário; `linhas` consome esse horário e `render` consome `linhas` (`bin/crons_duracao.py:76,93-99`; `app/config/crons.py:146-160,241-259,1791,1865-1866`).

   Os campos das entradas de cron são escritos por `C`, `RAW` e `ENC`; os leitores envolvidos incluem expansão/renderização, cálculo de sobreposição e placar (`app/config/crons.py:29-54,102-109,1785-1815`; `app/chamados/tests/test_contract_crons.py:348-367`; `app/core/management/commands/pipeline_placar.py:38-50,84-109`).

2. **Contraexemplo concreto.**

   Cadastro individual de colaborador e data que comprovem divergência de negócio em produção: `[nao verificado]`.

   Há contraexemplo concreto **de configuração**: no cenário de 05/11/2026, fazendo apenas a sombra caber com `4620 s`, como o próprio selo fabrica, permanece `calcular_scores` às **03:25**, com duração cadastrada de **310 s**, diante de `deploy.sh` às **03:30** (`app/core/tests/test_marca_cron15.py:69`; `app/config/crons.py:382,307`; `app/config/crons_duracao.json:14`).

   O teste arredonda essa duração para seis minutos, calcula término às **03:31** e acusa a sobreposição. Portanto, fazer a sombra caber não basta para esse marcado passar, contrariando a promessa de que cada um dos 15 passa (`app/chamados/tests/test_contract_crons.py:353-367`; `app/core/tests/marca_cron15.py:9-11`). Ocorrência efetiva dessa colisão em produção: `[nao verificado]`.

3. **Regra de negócio alterada sem lastro.**

   Alteração demonstrável de regra de negócio: `[nao verificado]`. O comportamento acrescentado controla prazo e classificação dos testes (`app/core/tests/marca_cron15.py:24-62`).

   Há lastro interno para o silenciamento temporário: prazo, tripwire e fila são exigidos pela L-062; o BACKLOG registra especificamente CRON-15, os 15 nomes, prazo e proibições (`app/docs/LEIS.md:96`; `app/docs/BACKLOG.md:747`). Confirmação externa do aval mencionado nesses registros: `[nao verificado]`.

4. **Selo que não morde — achado.**

   `test_a_unica_causa_e_a_sombra` troca somente a duração da sombra, confere quantidade de linhas e chama `render`. Não executa os 15 corpos nem verifica seus resultados (`app/core/tests/test_marca_cron15.py:67-71`).

   Assim, ele passa apesar da sobreposição do item 2 e também continua passando se o comportamento acrescentado pelo decorator for revertido para devolver o teste cru: esse selo não chama o decorator nem lê sua flag (`app/core/tests/test_marca_cron15.py:67-71`; `app/core/tests/marca_cron15.py:57-62`). Os testes de prazo verificam a flag separadamente; isso não transforma o render em prova de causa única (`app/core/tests/test_marca_cron15.py:73-85`).

   A lacuna contraria a exigência de caso que morde para selo com mock ou afirmação sobre saída (`app/docs/LEIS.md:97`; `CLAUDE.md:191`).

5. **Cadastro de hoje decidindo passado.**

   Nos caminhos examinados, não há leitura de `situacao`, `data_demissao`, posto, empresa cadastral, regime, `isento_ponto` ou vínculo atual de colaborador: o controle novo lê prazo e objetos de teste; seus consumidores trabalham com agenda e corridas de cron (`app/core/tests/marca_cron15.py:24-62`; `app/config/crons.py:1785-1815`; `app/core/management/commands/pipeline_placar.py:38-109`).

   Existe leitura da **agenda atual** para julgar corridas de um `--dia` passado, mas isso não demonstra leitura retroativa dos campos cadastrais pedidos (`app/core/management/commands/pipeline_placar.py:38-50,120-123`). Violação cadastral concreta e efeito financeiro: `[nao verificado]`. A autoridade histórica de vínculo está indicada em `app/docs/JUIZES.md:98-101`.

**VEREDITO: ACHADO**

1. `app/core/tests/test_marca_cron15.py:67-71` — **item 4** — selo de causa única permanece verde com falha independente da sombra e com o comportamento do decorator revertido. **Toca dinheiro: nao**; efeito financeiro indireto `[nao verificado]`.

## 05/11 15:18 · sessao c544dca7e7f3 · - · - · origem=Ronald (tmux)

1. **Bloqueia — snapshot e foto não formam uma transação em todos os escritores.** `gerar_foto_folgas` lê o defeito, apaga, recria e chama regeneração sem `atomic` próprio: `app/escala/folgas.py:169`, `:173`, `:174`, `:193`. Há chamadas sem transação envolvente em `app/colaboradores/views.py:212`–`:246` e `app/escala/management/commands/lavrar_foto_folgas.py:144`–`:170`. A transação da regeneração começa posteriormente, em `app/ponto/portas/celula.py:212`. Consequência inferida: falha entre escrita da foto e re-julgamento deixa cadastro corrigido e julgamento antigo; próxima entrada já encontra defeito falso (`app/escala/folgas.py:210`, `:219`).

2. **Bloqueia — `atomic` não serializa a transição mensal.** Lançamento, alteração, lote e migração tiram snapshot dentro de transação, mas sem trava mensal/vínculo: `app/escala/folgas.py:299`–`:306`, `:344`–`:349`, `:372`–`:388`, `:480`–`:492`. Dois lançamentos em datas diferentes podem ler defeito verdadeiro antes de qualquer commit e ambos solicitar o mês inteiro. A unicidade protege somente `(vínculo,data)`, não a transição mensal (`app/escala/models.py:1600`). Duplicação efetiva de perguntas/chamados sob concorrência: **[não verificado]**.

3. **Escritores explícitos de FolgaDia:**
   - Foto: delete e bulk-create, `app/escala/folgas.py:170`–`:177`.
   - Lançamento: get-or-create, `:302`–`:303`.
   - `_foto_do_dia`: delete/create/save tipo, `:323`, `:327`, `:331`–`:332`; chamados por alteração `:346` e lote `:379`.
   - Reparentar: troca FK/save, `:482`–`:483`.
   - Todos esses atos chegam à comparação mensal, exceto `_foto_do_dia` isoladamente, cujo contrato exige os atos completos (`:310`–`:314`).
   - `core/portas.py` é registro: família `app/core/portas.py:51`, módulo autorizado `:470`, consultas ao registro `:515`–`:526`; não é escritor de linhas.

4. **Escritores indiretos fora de folgas.py:** apagar EC apaga FolgaDia por CASCADE (`app/escala/models.py:1582`–`:1584`). Existem deletes de EC em `app/colaboradores/services/vinculo.py:864` e `:940`, sem snapshot mensal. Apagam o próprio vínculo, portanto não são a entrada de folga que transforma True→False no vínculo sobrevivente exigida pelo aval. A exclusão de calendário poderia modificar `calendario_origem` por SET_NULL (`app/escala/models.py:1586`–`:1588`); não encontrei delete explícito de FolgaCalendario na produção pesquisada **[não verificado quanto a exclusões externas]**.

5. **Leitores diretos de FolgaDia encontrados na produção:**
   - `app/escala/folgas.py:101`, `:165`, `:296`, `:317`, `:464`, `:502`.
   - Defeito/cálculo: `app/escala/models.py:1249`, `:1286`, `:1310`, `:1356`, `:1359`.
   - Carregadores: `app/escala/alimentacao.py:98`; `app/ponto/precedencia.py:206`.
   - Plano: `app/escala/views_plano_folgas.py:60`, `:112`.
   - Diagnósticos: `app/escala/services/fase_conflitante.py:74`; `app/ponto/services/diagnostico_escala.py:102`; `app/relatorios/furos.py:86`.
   - Comandos: `app/escala/management/commands/reparentar_folgas.py:28`; alias `_FD` em `app/chamados/management/commands/auditar_invariantes_chamados.py:1533`.

6. **Sem segundo juiz mensal encontrado:** `meses_no_defeito` delega a EC (`app/escala/folgas.py:210`); comparação posterior também (`:219`). Fonte decide ciclo, folga fixa e existência de foto no mês calendário (`app/escala/models.py:1246`–`:1250`). Deduplicação e enumeração de datas não refazem esse predicado (`app/escala/folgas.py:207`–`:221`).

7. **Contraexemplos e limites:**
   - Mudança normal de vínculo no meio do mês: novo laço aplica a fonte `vinculo_cobre_a_data` (`app/ponto/portas/celula.py:295`), cuja vigência está em `app/escala/alimentacao.py:214`–`:216`.
   - Feriado: regeneração usa cálculo existente (`app/ponto/portas/celula.py:249`), que chama predicado de feriado (`app/escala/models.py:1367`–`:1370`, `:1386`–`:1392`).
   - Última folga removida: snapshot anterior vazio; não re-julga o restante do mês (`app/escala/folgas.py:210`, `:217`–`:221`, `:345`–`:349`). O sentido inverso foi expressamente excluído no comentário do aval (`:250`–`:251`): **fila**, não nova exigência deste aval.
   - Lote cruzando meses: snapshot cobre todos elegíveis e deduplica por mês (`:207`–`:210`, `:373`); comparação ocorre depois de todas as escritas (`:385`–`:388`).
   - Reparentar: snapshot somente do receptor, antes da mudança de FK (`:481`–`:483`); passado usa `so_rejulgar` (`:490`–`:492`).

8. **Fila — testes deixam brechas relevantes:** teste de foto usa `TestCase` (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:26`), mascarando ausência de transação própria, e admite skip (`:172`–`:173`). Teste de lote usa um único mês (`:157`–`:163`). Teste reparentar usa 31/10 e hoje 05/11 com competência iniciando dia 21 (`:40`, `:186`–`:187`), logo não exercita `so_rejulgar=True`; eliminar esse ramo de `app/escala/folgas.py:492` não seria detectado por ele.

## 05/11 15:17 · sessao 84857165273e · - · - · origem=Ronald (tmux)

1. **Bloqueia — a condição “pai na fila” tem corrida entre leitura e escrita.** O comando materializa `na_fila` em `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`; escreve reversão em `:91`, recarrega pais em `:92` e só costura em `:95`. Não envolve esse trecho em transação/trava. `reapontar_resolvedora` abre transação em `app/chamados/services/fio.py:837`, e `_costura_nao_mata_o_fio` recarrega/trava o destino em `:759-763`, mas aceita qualquer status não terminal em `:764-766`; não pergunta novamente `qs_na_fila`. Logo, pai silenciado depois da seleção continua recebendo a pergunta. O escritor real do silêncio salva `silenciado_ate` em `app/chamados/services/arquivo.py:166-171`. Também vale pai convertido em `registrado`, pois a fila exige `COBRANCA` em `app/chamados/services/painel.py:186-188`, enquanto a costura só exclui terminais em `app/chamados/services/fio.py:764-766`. Ocorrência em produção [não verificado].

2. **Fila — ordem item 1 → reata não é demonstrada por código nem pelo dado.** `_rodar` começa diretamente pela leitura do C11 (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:54`) e executa a reata em `:95`; não possui verificação de conclusão do item 1. `app/config/crons.py:760-763` classifica o comando como ato único e ainda declara APPLY em produção bloqueado até corte de desenho. O dado fornecido não demonstra execução/deploy nessa ordem [não verificado]. Isso é falta de evidência de pouso, não prova de execução fora de ordem.

3. **Sem novo juiz ou escritor na alteração da reata.** A definição da fila continua em `app/chamados/services/painel.py:186-188`, agora consultada pelo comando (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:53-64`). A reata chama `app/chamados/services/limbo_folgas.py:237-244` → `app/chamados/services/fio.py:837-848`, que consulta `resolvedora_valida` em `:838` e relavra no mesmo save em `:845-848`. A guarda de turno está em `app/chamados/juizes.py:1903-1941`. O defeito é a ausência de revalidação atômica da condição de fila, não predicado duplicado (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:59`, `app/chamados/services/fio.py:764-766`).

4. **Censo de pai/fio:** não são campos literais de `PerguntaDisputa`; o vínculo persistido relevante é `chamado_destino` (`app/chamados/models.py:2772-2778`), com fallback para `disputa.chamado_id` em `app/chamados/services/fio.py:116-117`. Escritores:
   - nascimento e reapontamento por identidade em `PerguntaDisputa.abrir`, `app/chamados/models.py:2418-2466`;
   - reapontamento explícito em `app/chamados/services/fio.py:845-848`.
   
   O próprio contrato enumera esses dois arquivos em `app/core/tests/test_contract_escritores_estado.py:60-62`. O “pai LIMBO” é selecionado como o vivo de maior PK por colaborador em `app/chamados/services/limbo_folgas.py:212-220`; C11 publica seu ID e as perguntas candidatas em `app/escala/services/cadastro_realidade.py:457-463`. Leitores relevantes: C11 lê a âncora em `:423-448`; o comando lê destino anterior para reversão em `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:85-90`; o fio agrupa pelo mesmo `chamado_que_lista` em `app/chamados/services/fio.py:230-240`; gates de encerramento leem perguntas ancoradas em `app/chamados/models.py:328-350`. A reata idêntica retorna sem salvar em `app/chamados/services/limbo_folgas.py:239-242`.

5. **Relógio da reata:** o diff lê `timezone.now()` no comando, em `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:52`. O dia local vem desse instante em `app/escala/services/cadastro_realidade.py:489`; as competências vêm de `competencias_a_lavrar` em `:495`, e o intervalo é cortado em ontem em `:497`. A nova consulta de fila lê outro instante dentro de `qs_na_fila`, `app/chamados/services/painel.py:188`; portanto o comentário “UM relógio no ato” do comando não descreve todo o caminho (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:52-59`).

6. **Os testes têm mordida local, mas não cobrem estas mutações plausíveis:** [nao verificado]

| Teste | O que observa | Mutação que não pegaria | [nao verificado]
|---|---|---|
| Cadastro `caso_107` | mudança de veredito em 22/27, emissão real em 22, célula TXT intocada, uma trilha (`app/escala/tests/test_f3_beco1_cadastro_entrou.py:118-132`) | proteger apenas TXT e esquecer holerite publicado; a fixture usa só `ExportacaoDominio` (`:52-54`, `:100`). Também não conta duplicatas de pergunta/chamado (`:128-129`). |
| Cadastro `caso_155` | outubro muda; setembro permanece (`:137-143`) | remover guardas de célula humana/futuro; todas as fixtures são geradas e passadas (`:63-64`, `:137-139`). |
| Cadastro segunda folga | segunda entrada não rejulga nem acrescenta trilha (`:148-154`) | duplicar perguntas/chamados na primeira execução: não há contagem dessas entidades (`:148-154`). |
| Cadastro lote | dois dias mudam após uma folga (`:159-163`) | tirar snapshot só do primeiro mês do lote; usa apenas uma data de outubro (`:161`). |
| Cadastro foto | veredito de 25/10 muda (`:168-174`) | `gerar_foto_folgas` virar no-op retornando zero: o teste passa como `skipTest` (`:172-173`). | [nao verificado]
| Cadastro reparentar | vínculo novo recebe folga e dia 25 é rejulgado (`:180-188`) | remover a nova via de competência passada (`so_rejulgar`): usa folga 31/10 e hoje 05/11 (`:186-187`), com empresa iniciando competência no dia 21 (`:40-41`). |
| Reata apply | destino, reversão, saída de C11 e segunda rodada (`app/chamados/tests/test_f3_beco1_reata_limbo.py:53-65`) | gravar reversão **depois** da costura; verifica conteúdo final, não ordem (`:56-60`). |
| Reata dry | saída DRY e destino inalterado (`:71-74`) | dry escrever trilha ou relavrar pergunta mantendo destino; só destino é comparado (`:74`). |
| Reata sem pai | quantidade de chamados, destino e C11 (`:79-86`) | escrever log/trilha desnecessária para cada candidata sem pai; não inspeciona auditoria (`:79-86`). |
| Reata sem reversão/existente | exceções e destino inalterado (`:92-100`) | alterar outro campo da pergunta ou do chamado antes de levantar; só destino é comparado (`:100`). |
| Reata pai silenciado | silêncio já presente impede destino/reversão e mantém C11 (`:113-122`) | retirar a revalidação de fila no momento da escrita — já ausente; silêncio só ocorre antes de `_roda` (`:113-116`). Também não pega substituir fila por “silêncio não futuro”, admitindo `registrado`. |
| Reata silêncio vencido | reata e destino correto (`:132-135`) | duplicar perguntas/chamados ou não relavrar veredito no reapontamento: não compara contagens/lavra (`:134-135`). |

Execução dos testes, resultados RED/GREEN e mutações executadas [não verificado]. [nao verificado]

7. **Emissão existente conferida:** a chamada nova de rejulgamento usa `julgar_celula(... apply_=True, forcar=True)` em `app/ponto/portas/celula.py:402`; esse juiz chama `julgar_colab` em `app/ponto/services/cartorio.py:1988-1991`. O caminho aplicado chama a fábrica por célula julgada em `:981-997`; a fábrica direciona limbo entregue à Pauta em `app/chamados/services/fabrica_da_celula.py:171-173` e demais motivos à pergunta com push silenciado em `:174-176`. Portanto não encontrei, nesse encadeamento normal, novo caminho deliberado de “julgar sem emitir” (`app/ponto/portas/celula.py:402`, `app/ponto/services/cartorio.py:997`). Falha operacional do julgamento é capturada/logada e não derruba a correção em `app/ponto/portas/celula.py:405-413`; cobertura dessa falha pelos testes novos [não verificado].

## 05/11 15:17 · sessao 0f8f4d2250d7 · - · - · origem=Ronald (tmux)

1. **Selo sem mordida para a cura:** `test_MORDE_o_universo_e_o_mesmo_do_fora_da_folha` monta o esperado chamando `fora_da_folha` e compara somente universo/datas; descarta `_n` e `_p`. Com a cura revertida, o vigia volta a obter esses mesmos dados de `fora_da_folha`, portanto a igualdade continua por construção. É selo de invariância, não de alimentação única. Execução do mutante: `[nao verificado]`. Referências: `app/chamados/tests/test_n1_alimentacao_unica.py:109`, `:115`, `:117`, `:118`. **Item 4; toca dinheiro: não.**

2. **Há outros testes que medem a cura:** os testes de alimentação e consulta do app afirmam contagens iguais a um; os de sementes afirmam identidade da instância e ausência de consultas individuais. Logo, o achado anterior não demonstra ausência total de cobertura da cura. Referências: `app/chamados/tests/test_n1_alimentacao_unica.py:89`, `:92`, `:95`, `:97`, `:121`, `:137`, `:139`, `:148`. O teste `test_a_saida_do_dia_nao_muda` também ignora contagens e cobre invariância da saída: `:99`, `:101`, `:107`. Execução dos testes: `[nao verificado]`.

3. **Leitores fora do diff:** `montar_linhas` chama `fora_da_folha(com_itens=True)` e lê `itens`, `colabs`, `dias`, `datas` e `grupo`, produzindo `status`, `motivo`, `dono`, `tipo` e `saldo` do retrato: `app/folha/retrato_folha.py:1157`, `:1158`, `:1159`, `:1167`, `:1171`, `:1177`, `:1186`. O escritor persistente é `update_or_create` do comando: `app/folha/management/commands/lavrar_retrato_folha.py:74`. O comando do vigia lê `censo_alcance`, imprime e carimba sua saída: `app/chamados/management/commands/vigiar_fio_morto.py:168`, `:175`, `:217`, `:218`.

4. **Outro leitor de `_alimentar` usa cadastro atual para competência parametrizada:** `dias_sem_fio(mes, ano)` calcula a janela solicitada, mas seleciona células por `colaborador__empresa=emp` e `colaborador__situacao='ativo'`; depois chama `_alimentar` e lê `_fio_do_dia`. Referências: `app/ponto/services/pautas_dos_discordantes.py:46`, `:68`, `:70`, `:71`, `:75`, `:77`. Essa seleção pode excluir do levantamento histórico uma pessoa atualmente desligada ou transferida; caso real fornecido e regra temporal específica que proíba essa seleção: `[nao verificado]`. **Item 5; efeito financeiro concreto: `[nao verificado]`.**

5. **Regra nova no diff:** não identifiquei predicado novo de negócio na extração: `dias_que_seguram` continua delegando `classificar_export` e `_dias_que_retem`; `_fio_do_dia` continua lendo a porta do juiz. Referências: `app/folha/fora_da_folha.py:119`, `:137`, `:65`, `:77`, `:78`. O corte exige precisamente esse juiz compartilhado: `app/docs/CORTES.md:404`, `:409`; a autoridade do export está em `app/docs/JUIZES.md:74`. Equivalência dinâmica para todos os cadastros: `[nao verificado]`.

Limite dos selos novos: a fixture permanece em colaborador ativo, empresa/posto únicos e data específica; não ensaia mudanças históricas de situação, empresa, posto, regime ou isenção. Referências: `app/chamados/tests/test_n1_alimentacao_unica.py:24`, `:25`, `:38`, `:41`, `:43`.

## 05/11 15:17 · sessao 53166b9051da · - · - · origem=Ronald (tmux)

Rastreamento da porta:

| Valor | Escritor | Leitor |
|---|---|---|
| `alim['instancias']` | Sementes recebidas em `app/chamados/juizes.py:1244`; captura lazy em `app/chamados/juizes.py:992`; fallback em `app/chamados/juizes.py:1262` | Injeção em `disputa.colaborador`, `app/chamados/juizes.py:988`; `_no_app`, `app/chamados/juizes.py:1263` |
| `disputa.colaborador` em memória | `app/chamados/juizes.py:990` | Tranca em `app/chamados/juizes.py:930`; classificação da falta em `app/chamados/juizes.py:1145` |
| `alim['vivas']` | `pergunta_viva` e agrupamento em `app/chamados/juizes.py:1247` | Porta em `app/chamados/juizes.py:1323`; vigia em `app/chamados/services/vigia_fio.py:584` |
| `alim['no_app']` | `app/chamados/juizes.py:1264` | Porta em `app/chamados/juizes.py:1325`; vigia em `app/chamados/services/vigia_fio.py:595` |
| `_tranca_memo` | Criação em `app/chamados/juizes.py:969`; empresa/tranca em `app/chamados/juizes.py:938`; toque humano em `app/chamados/juizes.py:1025` | Tranca em `app/chamados/juizes.py:943`; toque em `app/chamados/juizes.py:1010` |
| `_pv_memo` | `app/chamados/juizes.py:845` | `app/chamados/juizes.py:834` |

As sementes vêm dos colaboradores retidos carregados por `dias_que_seguram`, do argumento de `o_que_segura` ou do `in_bulk` do vigia. Referências: `app/folha/fora_da_folha.py:131`, `app/folha/fora_da_folha.py:166`, `app/folha/fora_da_folha.py:218`, `app/chamados/services/vigia_fio.py:577`.

**Item 5 — empresa atual decidindo dia passado.** `dia_da_pergunta_trancado` toma `data_evento` da pergunta e consulta a tranca pela **empresa atual** de `disputa.colaborador`; não consulta histórico de lotação nesse caminho. Referências: `app/chamados/juizes.py:929`, `app/chamados/juizes.py:936`, `app/chamados/juizes.py:940`. O cadastro pode mudar posto e empresa juntos, registrando evento histórico datado: `app/colaboradores/views.py:1385`, `app/colaboradores/views.py:1392`. Uma transferência real afetada ou regra anexada que determine expressamente outra empresa para essa tranca: `[nao verificado]`.

**Item 5 — vínculo.** A classificação subsidiária da pergunta usa o vínculo **do dia**, passando pela autoridade de escala, e lê os marcos congelados da célula. Não há lastro para acusar leitura do vínculo ativo nesse caminho: `app/escala/servico_jornada.py:354`, `app/escala/servico_jornada.py:427`, `app/docs/JUIZES.md:98`.

**Item 2 — divergência adicional fora do diff.** `_dentro_da_janela_viva` chama as janelas atual e anterior sem empresa, usando o corte padrão 21; o cadastro por empresa existe. Referências: `app/chamados/juizes.py:561`, `app/chamados/juizes.py:580`, `app/ponto/janelas.py:50`. Isso contrasta com a consulta de competência por corte da empresa indicada em `app/docs/JUIZES.md:70`. Cadastro real de empresa com outro corte e pergunta afetada: `[nao verificado]`.

**Item 3.** Nesta cadeia, o diff semeia instâncias e mantém os mesmos predicados de vitalidade e porta; não identifiquei alteração demonstrável de regra de negócio sem lastro. Referências: `app/chamados/juizes.py:1244`, `app/chamados/juizes.py:1247`, `app/chamados/juizes.py:1325`; autoridade da porta em `app/docs/CORTES.md:401`.

Não há no anexo fornecido cadastro de produção com empresa anterior/atual, corte ou tranca que permita apresentar contraexemplo nominal dessa cadeia: `[nao verificado]`.

## 05/11 15:17 · sessao d998335e47b9 · - · - · origem=Ronald (tmux)

Rastreamento em leitura estática:

- `dias_que_seguram` obtém `itens` de `classificar_export`, seleciona somente `motivo == 'furo_espelho'`, carrega colaboradores atuais e calcula dias por `_dias_que_retem` (`app/folha/fora_da_folha.py:118`, `:128`, `:131`, `:137`). `fora_da_folha` lê esse mapa em `:161-175`; `censo_alcance` lê seus dias em `app/chamados/services/vigia_fio.py:559-561`.
- A extração conserva aquelas fontes; alteração de regra negocial introduzida especificamente pelo diff: `[nao verificado]`. Ocorrência em cadastro real de produção: `[nao verificado]`.

Leituras do cadastro atual decidindo competência passada:

| Campo | Escritor | Leitor e consequência |
|---|---|---|
| `situacao` | desligamento: `app/colaboradores/services/desligamento.py:104-108`; reativação `:210-211` | filtro de vínculo **atual** `ativa=True`, ciclo 12x36, colaborador atualmente ativo determina `travas_aprovacao` de qualquer competência: `app/folha/export.py:787-814`. |
| `data_demissao` | `app/colaboradores/services/desligamento.py:105-108` | qualquer data preenchida retira de **qualquer** competência com `rescisao_modulo_proprio`, sem comparação temporal: `app/folha/export.py:840-845`. Na retenção por célula, corta o fim do período: `:1182-1187`. |
| `posto` e `empresa` | transferência grava ambos: `app/colaboradores/views.py:1385-1388`; histórico separado `:1392-1399` | universo dos fechamentos usa empresa **atual**, não histórica: `app/folha/export.py:767`. `o_que_segura` também toma empresa atual para a janela: `app/folha/fora_da_folha.py:212-215`. |
| `regime_trabalhista` | formulário sem parâmetro de vigência: `app/colaboradores/views.py:512-523` | regra de feriado para competência passada usa `colaborador.empresa` e regime **atuais**: `app/core/regua_cct.py:352-366`, alcançada por `app/folha/export.py:976-978` → `:359-369`. |
| `isento_ponto` | passe livre: `app/colaboradores/views.py:173-176`; trilha `:183-186` | juiz lê apenas flag atual: `app/core/constants.py:13-18`; retenção retorna vazia antes de ler células passadas: `app/folha/export.py:1174-1178`. Eventos do fechamento também ficam vazios: `:406-408`. |
| vínculo | criação/alteração: `app/colaboradores/services/vinculo.py:266-280` | **Não generalizar como vínculo atual:** ciclo da competência usa vínculo do último dia da janela: `app/folha/export.py:346-349`; praça/ciclo da régua também usam vínculo do dia: `app/core/regua_cct.py:328-339`. O filtro atual da trava de aprovação é a exceção acima: `app/folha/export.py:787-814`. |

Contracenário **construído**, sem atribuir ocorrência a produção: colaborador admitido em 01/01/2026, código Domínio preenchido, fechamento 09/2026 existente, célula de 01/09/2026 com furo retentor; desligamento em 25/10/2026. Ao consultar 09/2026 hoje, `classificar_export` retorna `rescisao_modulo_proprio` pela demissão de outubro (`app/folha/export.py:840-845`), e `dias_que_seguram` não inclui a pessoa porque seleciona somente `furo_espelho` (`app/folha/fora_da_folha.py:128-137`). O comentário da regra do próprio export diz “demissao na janela” (`app/folha/export.py:834-839`); uma LEI/CORTE anexada que determine explicitamente o tratamento dessa demissão posterior: `[nao verificado]`.

Portanto o item 5 tem leituras atuais comprovadas, mas não basta alegar que cada uma viola regra: a fonte de isenção é expressamente a flag (`app/core/constants.py:15-18`), enquanto as leis gerais permitem cálculo com o cadastro disponível (`app/docs/LEIS.md:149`). A regra L-061 exige consumidor da célula/ata (`app/docs/LEIS.md:95`), e a soberania possui exceção de correção cadastral pela porta (`app/docs/LEIS.md:71`). Não há dado raspado de colaborador real para fechar contraprova de produção `[nao verificado]`.

## 05/11 15:16 · sessao b426681d7d59 · REVISAO-HE · verificador · origem=Code

Há achados nos leitores alcançados pela revisão, principalmente cadastro atual decidindo passado. A alteração do mapa preserva os ramos de leitura e fallback; regressão financeira causada especificamente pelo diff: [nao verificado]. (`app/escala/models.py:1087`, `app/escala/models.py:1117`, `app/escala/models.py:1182`, `app/escala/models.py:1227`.)

A análise é estática. Execução dos testes, identidade do commit/primeiro pai e ocorrência dos cenários em produção: [nao verificado]. [nao verificado]

No **item 1**, o campo novo é estado transitório na instância; os demais valores abaixo são insumos ou resultados que o diff passa a alcançar pelo contexto. (`app/escala/alimentacao.py:43`, `app/ponto/services/fechamento.py:269`.)

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `_celulas_do_ato = (ini, fim, celulas)` | Contexto instala, restaura ou remove: `app/escala/alimentacao.py:43`, `:49`, `:51`. | `_celulas_do_ato_para` limita pela janela: `app/escala/models.py:1063`, `:1067`; quatro métodos consultam em `:1088`, `:1118`, `:1183`, `:1228`. |
| Mapa `(colaborador_id, data) → CelulaDia` | Carregador: `app/escala/alimentacao.py:73`; fechamento: `app/ponto/services/fechamento.py:591`; espelho: `app/ponto/services/espelho.py:1049`. | Contexto: `app/ponto/services/fechamento.py:269`; motor recebe o mesmo mapa em `app/ponto/services/fechamento.py:732` e `app/ponto/services/espelho.py:1131`. |
| `trabalha`, `escala_geradora`, `dna` | Geração/regeneração: `app/ponto/portas/celula.py:247`, `:260`, `:593`, `:612`; reversão: `:530`, `:532`, `:535`. | Trabalho: `app/escala/models.py:1095`; vínculo do dia: `app/escala/alimentacao.py:269`; DNA nos leitores abaixo. |
| `dna.marcos`, pausas, `extra_declarada` | `montar_dna` congela o template: `app/ponto/management/commands/gerar_celulas.py:81`, `:82`, `:92`, `:122`. | Marcos: `app/escala/models.py:1124`; pausas: `:1189`; extra: `:1235`; primeira pausa também via `:1163`. |
| Trabalho previsto e marcos consumidos | Retornos dos métodos anteriores. | Cursor do fechamento: `app/ponto/services/fechamento.py:713`; janela de HE: `app/ponto/motor_calculo_v2.py:1524`; pontualidade: `:2041`; extra no espelho: `app/ponto/services/espelho.py:733`. |
| `motor`, impressão/fonte da régua, régua por dia, `r_fatia` | Dentro do novo contexto: `app/ponto/services/fechamento.py:270`, `:272`, `:274`, `:276`, `:277`. | Composição: `:285`, `:289`, `:302`; lavratura: `:1205`; carimbo no espelho: `app/ponto/services/espelho.py:1296`. |

No **item 2**, há este contraexemplo **construído a partir do cadastro do teste**, sem atribuí-lo à produção: [nao verificado]

O cadastro HEIM tem template 07–16, DNA 08–17 e batidas nos dias **01, 02 e 05/10/2026**; a expectativa declarada é de **três pontas de dez minutos**. (`app/ponto/tests/test_he_pendente_identity_map.py:33`, `:77`, `:84`, `:94`, `:109`.)

Considere esse mesmo cadastro desligado em **21/10/2026**, com releitura da competência **10/2026 em 05/11/2026**. O escritor do desligamento grava `situacao='desligado'` e a data; `he_pendente_lavrado.apurar` então exclui o colaborador inteiro pelo estado atual. Resultado deduzido: seus três dias deixam de integrar o retrato. (`app/colaboradores/services/desligamento.py:104`, `app/ponto/services/he_pendente_lavrado.py:89`, `:92`.)

O fechamento mantém esse colaborador no universo, pois a demissão é posterior ao início da janela. O corte anexado retira desligados com demissão **anterior** à janela e exige um sítio para o predicado do universo; a L-095 exige leitura da mesma lavratura. A divergência está na seleção dos participantes, antes da conta dos minutos. (`app/ponto/services/fechamento.py:423`, `:430`, `app/docs/CORTES.md:270`, `app/docs/LEIS.md:113`.) Execução desse contrafactual e existência de vítima real: [nao verificado].

No **item 3**, não demonstrei regra de negócio nova introduzida pelo mapa. Sob o contrato de carga completa, os chamadores fornecem o período e os quatro leitores mantêm os retornos anteriores; fora da janela, permanece a consulta ao banco. Isso conserva a autoridade documentada dos marcos congelados. (`app/ponto/services/fechamento.py:575`, `:591`, `app/ponto/services/espelho.py:1049`, `app/escala/models.py:1067`, `:1094`, `:1124`, `:1202`, `:1234`, `app/docs/JUIZES.md:22`.)

No **item 4**, dois testes de valor passariam com a integração da cura revertida: [nao verificado]

- `test_o_retrato_e_o_da_REGRA_pela_celula`: o próprio arquivo declara que a ponta do retrato nasce fora de `apurar_por_fatia`. (`app/ponto/tests/test_he_pendente_identity_map.py:20`, `:109`.)
- `test_a_apuracao_do_motor_e_a_da_REGRA_pela_celula`: sem contexto, `marcos_do_dia` consulta o mesmo DNA no banco; os números esperados continuam iguais. (`app/ponto/tests/test_he_pendente_identity_map.py:149`, `app/escala/models.py:1122`, `:1124`.)

O `MORDE` substitui o mapa por `{}`, inventando ausência de célula; isso força o template e difere de reverter a cura, que recupera a célula do banco. Há, entretanto, um selo específico de consultas que deve distinguir a reversão por custo. Portanto, esses testes de valor não certificam isoladamente a cura, mas isso não torna a suíte inteira vazia. (`app/ponto/tests/test_he_pendente_identity_map.py:116`, `:161`, `:168`, `app/escala/models.py:1133`.) Resultados executados: [nao verificado].

No **item 5**, o rastreio dos sete campos pedidos ficou assim:

| Cadastro | Escritor encontrado | Uso sobre passado |
|---|---|---|
| `situacao`, `data_demissao` | Desligamento: `app/colaboradores/services/desligamento.py:104`; reativação limpa a demissão: `:210`. | HE exige ativo atual em `app/ponto/services/he_pendente_lavrado.py:92`; fechamento considera a data em `app/ponto/services/fechamento.py:423`, `:430`. |
| `posto` | Transferência: `app/colaboradores/views.py:1385`; histórico datado: `:1392`. | Município do posto atual decide feriados do período: `app/core/feriados.py:113`; alimentação do fechamento: `app/ponto/services/fechamento.py:629`, `:720`; espelho: `app/ponto/services/espelho.py:147`, `:1030`. |
| `empresa` | Transferência: `app/colaboradores/views.py:1386`. | Empresa atual filtra a competência e determina sua janela: `app/ponto/services/he_pendente_lavrado.py:92`, `app/ponto/services/fechamento.py:435`, `:607`. |
| Regime da empresa | Formulário e persistência: `app/colaboradores/views.py:512`, `:521`, `app/colaboradores/services/estrutura.py:17`. | Para a data passada recebida, lê `empresa.regime_trabalhista` atual e escolhe CLT/CCT: `app/core/regua_cct.py:352`, `:353`, `:354`, `:366`. |
| `isento_ponto` | Passe livre: `app/colaboradores/views.py:176`. | Juiz sem data: `app/core/constants.py:13`; elimina furos do período em `app/ponto/services/fechamento.py:357` e todos os eventos do fechamento em `app/folha/export.py:407`. O comentário reconhece snapshot anterior à isenção em `:402`. |
| Vínculo | Vigência escrita pela porta: `app/colaboradores/services/vinculo.py:561`, `:797`. | A apuração principal consulta geradora/vigência do dia, não simplesmente o ativo atual: `app/ponto/services/fechamento.py:191`, `app/core/regua_cct.py:329`, `app/docs/JUIZES.md:98`. |

Essas leituras atuais antecedem as linhas modificadas no patch fornecido. Autorização específica nos anexos para cada efeito retrospectivo de posto, empresa, regime e isenção: [nao verificado]. A seleção histórica do vínculo, por sua vez, tem autoridade expressa nos cortes. (`app/docs/CORTES.md:284`, `:287`.)

**VEREDITO: ACHADO**

1. `app/ponto/services/he_pendente_lavrado.py:92` — **itens 2 e 5**: estado atual apaga do retrato dias de competência em cujo universo o fechamento mantém o colaborador. **Toca dinheiro: sim**, pela fila de decisão de HE e sua relavratura (`app/ponto/portas/he.py:202`); impacto real: [nao verificado].
2. `app/core/feriados.py:113` — **item 5**: posto atual alimenta feriados de período passado. **Toca dinheiro: sim** (`app/ponto/services/fechamento.py:720`); vítima real: [nao verificado].
3. `app/core/regua_cct.py:352` — **item 5**: empresa/regime atuais escolhem régua para data passada. **Toca dinheiro: sim** (`app/core/regua_cct.py:354`, `:366`); vítima real: [nao verificado].
4. `app/folha/export.py:407` — **item 5**: isenção atual suprime eventos de fechamento, inclusive snapshot anterior à isenção. **Toca dinheiro: sim** (`app/folha/export.py:402`, `:408`); ocorrência real: [nao verificado].
5. `app/ponto/tests/test_he_pendente_identity_map.py:109`, `:149`, `:161` — **item 4**: testes de valor preservado e mutante vazio não demonstram reprovação da reversão. **Toca dinheiro: nao**; existe teste específico de consultas em `:116`.

## 05/11 15:16 · sessao 0f845554ea44 · A-683-SEM-FIO · leitura · origem=Code

**O dado fornecido sustenta 416 em PROD e 418 na sombra; não permite atribuir “683 sem fio” a uma medição específica.** São números dos anexos, sem reexecução ou conferência em arquivo local: [não verificado]. O código distingue expressamente `censo()` — chamado vivo — de `censo_alcance()` — dia que retém a folha sem porta alcançável. (`app/chamados/services/vigia_fio.py:492–498`)

Abaixo, **certeza alta** significa mecanismo demonstrado pelo código; autoria e cronologia dos registros continuam **[não verificado]** quando faltam no anexo. Os testes citados fornecem contraexemplos escritos; sua execução nesta análise é [não verificado]. [nao verificado]

**1. De onde poderia sair 683**

| Candidato | Universo e unidade | Resultado pelo anexo | Contraexemplo / certeza |
|---|---|---|---|
| `censo_alcance()['fio_morto_alcance']` | Dias distintos `(colaborador, data)` que retêm a folha, nas competências enumeradas, após portas, teto e baldes legais. (`app/chamados/services/vigia_fio.py:580–587`, `:641–698`) | PROD **416/2036**; sombra **418/1987**. [não verificado] | Dia com Resolver oferecido sai para balde próprio, mesmo sem porta da fila/pergunta. Certeza alta da definição. (`app/chamados/tests/test_o261e_f5_alcance.py:219–223`) |
| Contagem bruta de dias **sem porta**, antes das exceções | Mesmo universo, contando `porta_alcancavel_do_dia(...) is None`; equivalente ao fio `nada` no recorte correspondente de `fora_da_folha`. (`app/chamados/services/vigia_fio.py:634–659`; `app/folha/fora_da_folha.py:76–78`, `:177–180`) | PROD **686**; sombra **675**, por soma dos anexos. [não verificado] | Inclui dias com decisão pendente ou ausência aguardando decisão. Não é o contador alarmado. Certeza alta da diferença. (`app/chamados/services/vigia_fio.py:305–313`) |
| Sem porta bruto, excluindo hoje/futuro | Mesmo conjunto anterior, apenas passado. (`app/chamados/services/vigia_fio.py:647–655`) | PROD **671**; sombra **675**. [não verificado] | Ainda inclui os quatro baldes legais. (`app/chamados/services/vigia_fio.py:651–659`) |
| `censo()['fio_morto']` e suas aberturas | Células acusando, apontadas por chamado **vivo**, com recorte de módulos de furo; não parte da folha. (`app/chamados/services/vigia_fio.py:334–378`, `:448–473`) | Valor não fornecido. **683: [não verificado]** | Terminal com pergunta viva pode ficar fora desse universo. Certeza alta dessa limitação. (`app/chamados/services/vigia_fio.py:494–498`) |
| `pergunta_no_app.medir()['total']` | **Chamados**, de colaborador ativo, em `COBRANCA`, nos módulos indicados, sem pergunta correspondente no app; não dias retidos. (`app/chamados/services/pergunta_no_app.py:119–145`) | **683: [não verificado]** | Dois chamados do mesmo dia podem gerar duas unidades; dia sem chamado não entra nessa consulta. (`app/chamados/services/pergunta_no_app.py:126–144`) |
| `viva_em_disputa_fechada()` | **Perguntas** vivas em disputas fechadas; conta também disputas distintas. (`app/chamados/services/vigia_fio.py:736–758`) | **683: [não verificado]** | Uma célula pode ter várias perguntas; disputa fechada também não equivale a chamado terminal. (`app/chamados/services/vigia_fio.py:747–758`; `app/chamados/services/fio.py:116–117`) |
| `vigia_carimbo.censo()` | **Perguntas de hora/data_hora validadas**, classificadas pelo registro efetivo; não dias sem porta. (`app/chamados/services/vigia_carimbo.py:97–116`, `:119–129`) | **683: [não verificado]** | Validação já mata a pergunta no primeiro degrau do C6; portanto “sem gravação” e “sem porta” são perguntas diferentes. (`app/chamados/juizes.py:1077–1079`) |

As contas que reconciliam os baldes principais são:

- **PROD:** `416 + 238 + 13 + 2 + 2 + 15 = 686` sem porta bruta; retirando os baldes e hoje/futuro, restam **416**. Valores dos anexos: [não verificado]. A ordem das retiradas está em `app/chamados/services/vigia_fio.py:641–659`.
- **Sombra:** `418 + 238 + 17 + 0 + 2 + 0 = 675`; após as retiradas, **418**. Valores dos anexos: [não verificado]. Mesma implementação: `app/chamados/services/vigia_fio.py:641–659`.

**Nenhuma soma de subconjuntos dos oito baldes principais, sem sobreposição, dá 683 nos anexos.** Aritmética sobre os valores fornecidos: [não verificado].

Existe uma soma numericamente exata: **`416 + 238 + 29 = 683`**. Mas ela mistura decisão pendente com fio morto e **conta a classe c duas vezes**, pois os 29 já pertencem aos 416. É um candidato a erro de consolidação, **não uma origem demonstrada do laudo**. (`app/chamados/services/vigia_fio.py:656–675`, `:694–695`; valores e autoria da soma: [não verificado])

**2. Causa e porta de cada classe**

“Causa” no censo é uma **classificação do estado observado**, em ordem exclusiva; não identifica automaticamente o escritor histórico. A escada vem depois das portas e dos baldes legais. (`app/chamados/services/vigia_fio.py:641–698`)

- **a — chamado vivo com véu.** O impedimento é a diferença entre `VIVOS` e a fila: `registrado` é vivo, mas não pertence a `COBRANCA`; silêncio vigente também exclui da fila. Os dois anexados têm detalhe `registrado`: [não verificado]. O nascimento pode escrever esse estado por `_evento_nascimento` → `REGISTRAR`; qual emissor produziu esses dois registros não consta do anexo. (`app/chamados/catalogo/motor.py:24–31`, `:130–134`; `app/chamados/models.py:894–899`, `:1007–1012`; `app/chamados/services/painel.py:186–188`)  
  **Porta que satisfaria o juiz:** chamado da célula na fila, ou pergunta viva no app sob âncora na fila. Isso não autoriza transformar indiscriminadamente registros de auditoria em cobranças. **Contraexemplo:** aberto sem silêncio dá porta; registrado dá a. **Certeza alta do mecanismo; autoria [não verificado].** (`app/chamados/juizes.py:1317–1329`; `app/chamados/tests/test_o261e_f5_alcance.py:134–159`)

- **b — respondida sem resposta.** `respondida_em` está preenchido e o texto é vazio; a porta rejeita explicitamente esse par. A resposta humana atual recusa vazio e grava texto/carimbo juntos. O ramo de fechamento sem credor documenta o escritor antigo que carimbava sem texto; hoje usa `carimbar(resposta='responder')`, condicionado a texto. Atribuir esse passado à célula 107534 seria **especulação [não verificado]**. (`app/chamados/juizes.py:1191–1199`, `:1322`; `app/chamados/services/disputa_emissao.py:1254–1256`, `:1282–1301`, `:1668–1675`; `app/chamados/models.py:2576–2583`)  
  **Porta esperada:** pergunta novamente respondível, pelo caminho de correção, sob âncora na fila; não inventar uma resposta. **Contraexemplo:** pendente sem carimbo sob aberto é porta; carimbada vazia dá b. **Certeza alta do impedimento; escritor individual [não verificado].** (`app/chamados/services/disputa_emissao.py:1393–1413`; `app/chamados/tests/test_o261e_f5_alcance.py:141–167`)

- **c — respondida com âncora terminal.** Há pergunta viva respondida, mas a âncora está terminal e não entra na fila. A âncora é `chamado_destino`; sem ele, o chamado da disputa. O autor da terminalização não é identificado pela classificação. (`app/chamados/services/vigia_fio.py:674–675`; `app/chamados/services/fio.py:116–117`; `app/chamados/services/painel.py:186–188`)  
  **Porta esperada:** validação da resposta sob âncora acionável na fila, preservando a identidade do fato; o reapontamento passa por `resolvedora_valida` e pela guarda da costura. **Contraexemplo:** respondida sob terminal dá c; sob âncora aberta pode chegar a `validar`. **Certeza alta do mecanismo; fechamento histórico [não verificado].** (`app/chamados/services/fio.py:836–848`; `app/chamados/tests/test_o261e_f5_alcance.py:169–174`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:167–172`)

- **d — colaborador na CxR, dia fora.** Ao chegar nesse ramo, não há pergunta viva nem chamado vivo que tenha produzido a; a pessoa aparece na lista, mas o dia não aparece em `dias_na_lista`. Assinatura do vínculo inteiro não endereça um dia. A causa demonstrada é **falta de destino por dia**, não “cadastro errado nesse dia”. (`app/chamados/services/vigia_fio.py:663–677`; `app/escala/services/cadastro_realidade.py:736–755`)  
  **Quem deixou de emitir ou matou o fio:** [não verificado]. A fábrica pode não gerar motivos por dia sem trabalho, colaborador não ativo, teto, ausência parcial destinada à lista ou deduplicação de pergunta histórica; esses insumos não vieram no anexo. (`app/chamados/services/fabrica_da_celula.py:89–126`)  
  **Porta esperada:** ação endereçada ao dia — fila/pergunta, ou presença do próprio dia na lista com dono BATIDA/CADASTRO. A tela cadastral concreta depende da assinatura ausente do anexo. **Contraexemplo:** dia listado BATIDA sai para `fio_do_admin`; dia listado ESTRUTURA permanece, mas é e, não d. **Certeza alta da classificação; causa histórica e tela específica [não verificado].** (`app/chamados/juizes.py:1317–1329`; `app/chamados/services/vigia_fio.py:221–227`; `app/escala/services/cadastro_realidade.py:449–527`; `app/chamados/tests/test_o261e_f5_alcance.py:194–203`)

- **e / pergunta viva sob chamado terminal.** Pelo menos uma pergunta viva tem âncora terminal, após não satisfazer a/b/c/d. Isso não significa que todas as perguntas da célula tenham o mesmo estado. (`app/chamados/services/vigia_fio.py:668–687`)  
  **Porta esperada:** questionário alcançável sob âncora na fila; pergunta viva sozinha não basta. **Contraexemplo:** pergunta pendente sob resolvido dá e/terminal; sob aberto na fila dá porta. **Certeza alta do mecanismo; origem dos 170 [não verificado].** (`app/chamados/juizes.py:1320–1325`; `app/chamados/tests/test_o261e_f5_alcance.py:141–147`, `:176–182`)

- **e / sem pergunta viva.** É o resíduo sem vivas que não satisfez d nem os ramos anteriores. Não informa se nunca houve pergunta ou se uma pergunta morreu. Os 18 registros do col950 constam assim no anexo: [não verificado]. (`app/chamados/services/vigia_fio.py:676–685`)  
  **Porta esperada:** chamado da célula na fila ou emissão/restabelecimento legítimo de pergunta; a fábrica escreve por `perguntar_pela_celula` → `perguntar` → geração. Qual guarda impediu esses 18 dias é [não verificado]. **Contraexemplo:** o mesmo dia sem pergunta passa a ter porta quando há chamado da célula na fila. **Certeza alta da ausência observada; causa histórica [não verificado].** (`app/chamados/services/fabrica_da_celula.py:129–176`; `app/chamados/tests/test_o261e_f5_alcance.py:126–138`)

**3. Por que os 185 d recebem ESTRUTURA**

O juiz executa esta precedência: defeito cadastral explícito → CADASTRO; batida fora do turno ou contagem ímpar → BATIDA; fase deslocada → CADASTRO; dia na CxR do motor → CADASTRO; restante → ESTRUTURA. Zero batidas é par e pode cair no resíduo. (`app/ponto/management/commands/e6_oraculo.py:72–75`, `:98–110`)

**A lista consultada pelo juiz não é a lista completa da tela.** `InsumosDoDono.cxr()` lê `resultado.dias_cadastro_x_realidade` do motor. `dono()` acrescenta as horas, a fase lavrada e `cadastro_do_dia_carregado`. Já d usa presença da pessoa na lista da tela e ausência do dia nela. (`app/ponto/management/commands/e6_oraculo.py:150–178`; `app/chamados/services/vigia_fio.py:676–677`)

**Contraexemplo direto:** `dono_da_divergencia([], False)` devolve ESTRUTURA; com o dia na CxR do motor, `([], True)` devolve CADASTRO. A própria classe d do anexo contém os três donos: 185/9/3. (`app/ponto/management/commands/e6_oraculo.py:98–110`; distribuição: [não verificado])

**Veredito:** a coexistência d + ESTRUTURA é compatível com a implementação e não demonstra que o juiz leu algo errado. A L-099 aponta esse juiz e não diz que toda presença do colaborador na lista torna todos os seus dias CADASTRO. (`CLAUDE.md:176`)

Entretanto, **não está demonstrado que os 185 resultados estejam corretos individualmente**: faltam horas, flags cadastrais, dias do motor e lavra de fase. [não verificado]. Além disso, o juiz foi definido para o dono da **divergência do dia**; usar seu resultado como prova de quem causou o defeito de roteamento excede sua definição. (`app/ponto/management/commands/e6_oraculo.py:55–70`)

**4. Quem escreve o terminal e o que F1 impede**

O núcleo atual é **`ChamadoColaborador._gravar_estado`**: atribui `status_local`, carimba encerramento/via, registra a transição e salva. (`app/chamados/models.py:195–239`, `:274–279`)

| Entrada | Caminho de escrita |
|---|---|
| Ato humano | `transicionar` → `_aplicar` → núcleo. (`app/chamados/models.py:159–193`, `:430`) |
| Reconciliação do sistema | `reconciliar` → decisão de fechamento → núcleo. (`app/chamados/models.py:607–620`) |
| Cancelamento | `cancelar` → F1 → núcleo. (`app/chamados/models.py:736–751`) |
| Cascata/encerramento | `encerrar` → F1 → núcleo. (`app/chamados/models.py:1524–1538`) |
| Tranca de competência | `encerrar_por_competencia_trancada` → F1 → núcleo. (`app/chamados/models.py:1541–1558`) |

**Sim, existe F1.** Para pergunta viva ancorada em competência cuja entrega responde **False**, o sistema tenta reatar a sobrevivente legítima; se não consegue, recusa o fechamento. O humano recebe recusa. O núcleo ainda possui uma guarda adicional para vivo → terminal. (`app/chamados/models.py:159–165`, `:215–221`, `:332–377`, `:393–428`)

**Mas a fotografia dos 170 não prova uma passagem vivo → terminal com pergunta já viva.** Há contraexemplos concretos:

- **Nascimento posterior:** pergunta nova pode nascer apontando para terminal; o `objects.create` novo não passa pela guarda da costura, diferentemente do reapontamento de existente. Isso está explicitamente declarado fora do recorte F1. (`app/chamados/models.py:2427–2466`; `app/chamados/services/fio.py:742–750`)
- **Vitalidade posterior:** destrancar ou uma célula voltar a acusar pode devolver vitalidade por leitura. (`app/chamados/juizes.py:917–924`, `:1123–1126`)
- **Terminal → terminal:** não é a passagem cercada; há teste explícito aceitando resolvido → fechado com pergunta viva. (`app/chamados/models.py:215–221`, `:400`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:211–216`)
- **Fronteira da competência:** entregue ou indeterminável fica fora de `_vivas_entre`; isso não prova que algum dos 170 esteja nessa condição. (`app/chamados/models.py:347–348`; ocorrência nos anexos: [não verificado])

Há ainda uma diferença entre **F1 e alcance**: o sobrevivente C8 é selecionado em `VIVOS`, sem o filtro de silêncio de `qs_na_fila`. Logo, reatar a um vivo não demonstra, por si, alcance pela fila. (`app/chamados/models.py:373–377`; `app/chamados/juizes.py:1836–1844`; `app/chamados/services/painel.py:186–188`)

**Quem escreveu os terminais específicos dos 170: [não verificado].** O anexo não fornece âncora, `via_encerramento`, instante nem trilha da transição; o núcleo grava justamente esses elementos para distinguir os caminhos. (`app/chamados/models.py:239`, `:275–278`)

**5. Quem escreve e quem lê os campos da decisão de porta**

A decisão direta usa **fila da célula**, ou **pergunta viva da célula + âncora na fila + presença no app + ausência de resposta vazia carimbada**. Resolver dia é um balde separado. (`app/chamados/juizes.py:1309–1329`)

| Campo/insumo | Quem escreve | Quem lê |
|---|---|---|
| Chamado `status_local` | Nascimento em `abrir/criar`; mudanças pelo núcleo. (`app/chamados/models.py:894–899`, `:932`, `:1048`, `:222–279`) | Fila e estados das causas. (`app/chamados/services/painel.py:155`, `:186–188`; `app/chamados/services/vigia_fio.py:606–617`, `:661–687`) |
| Chamado `silenciado_ate` | `arquivo.silenciar` atribui; `levantar` limpa. (`app/chamados/services/arquivo.py:160–171`, `:198–208`) | Exclui silêncio vigente; também altera verbo pelo relógio. (`app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:44–52`) |
| Chamado `celula_id` | Nascimento resolve a célula; `apontar_celula` liga existente ainda sem célula. (`app/chamados/models.py:913–932`, `:1026–1048`; `app/chamados/services/vinculos.py:92–99`) | Alimentação indexa chamados por célula. (`app/chamados/juizes.py:1232–1237`, `:1318`) |
| Pergunta `celula`, `data_evento`, `motivo`, `tipo_resposta`, `disputa`, `chamado_destino` | `PerguntaDisputa.abrir`; reapontamento também em `fio.reapontar_resolvedora`. (`app/chamados/models.py:2387–2466`; `app/chamados/services/fio.py:831–848`) | C6, seleção por célula, identidade do marco e âncora. (`app/chamados/juizes.py:1041–1054`, `:1242–1245`; `app/chamados/services/fio.py:116–117`) |
| Disputa `colaborador`, `chamado` | Nascimento da disputa. (`app/chamados/services/disputa_emissao.py:523–527`) | App seleciona pela pessoa; sem destino, o pai é a âncora. (`app/chamados/services/pergunta_no_app.py:46–53`; `app/chamados/services/fio.py:116–117`) |
| `resposta_colab`, `respondida_em` | Resposta humana, automática, reparo pela trilha e reabertura; `carimbar` pode completar carimbo com texto. (`app/chamados/services/disputa_emissao.py:1282–1301`, `:1393–1446`; `app/chamados/models.py:2576–2579`) | C6, veto de resposta vazia e pré-filtro do app. (`app/chamados/juizes.py:1092–1104`, `:1196–1199`, `:1322`; `app/chamados/models.py:2735–2748`) |
| `validada_em` | Validação; `carimbar(resposta='validar')`; resposta nova e reabertura desfazem. (`app/chamados/services/validacao.py:127–133`; `app/chamados/models.py:2580–2583`; `app/chamados/services/disputa_emissao.py:1295–1297`, `:1393`) | Primeiro veto do C6 e pré-filtro do app. (`app/chamados/juizes.py:1077–1079`; `app/chamados/models.py:2746`) |
| `materializada_em`, `via_resolucao` | Portas `carimbar`, `carimbar_identidade`, `descarimbar` e lote. (`app/chamados/models.py:2545–2549`, `:2575`, `:2614–2653`) | C6 examina tranca, identidade, marco e lastro; carimbo sozinho não equivale a morto. (`app/chamados/juizes.py:1087–1088`, `:1112–1126`, `:1149–1156`) |
| Célula `veredito`, `veredito_via`, `protestos`, `ata` | Porta da célula; cartório fornece lavra/ata; admin usa a mesma porta. (`app/ponto/portas/celula.py:735–783`; `app/ponto/services/cartorio.py:1802`; `app/ponto/services/veredito_celula.py:330`) | C6 lê decisão admin, acusação e marcos; retenção também lê veredito/protestos. (`app/chamados/juizes.py:1089–1091`, `:1123–1138`; `app/folha/export.py:1215–1236`) |
| Célula `dna.marcos`; ata `lampadas`, `luz`, `pendente` | Geração/regeneração grava DNA; lavra grava ata. (`app/ponto/portas/celula.py:251–264`, `:735–783`) | Juiz do marco, concordância da resposta e teto do C6. (`app/chamados/juizes.py:891–914`, `:1047–1054`, `:1136–1138`) |
| Batida: fato, vínculo à pergunta e retratação | Registro; anotação; retratação pela porta. (`app/ponto/registro_batida.py:94–157`, `:235–274`) | Lastro procura batidas plantadas não retratadas e slot esperado. (`app/chamados/juizes.py:716–723`) |
| Tranca: empresa/competência, `fechado_em`, `reaberto_em` | `trancar_periodo` e `reabrir_periodo`. (`app/ponto/services/fechamento.py:1391–1403`, `:1420–1425`) | `periodo_trancado` lê `reaberto_em`, não `ativo`; C6 consulta a competência da pergunta. (`app/ponto/services/fechamento.py:1448–1454`; `app/chamados/juizes.py:929–949`) |
| Toque humano após tranca | Cobrar dia, resolver dia e desarquivar escrevem trilha. (`app/chamados/services/cobrar_dia.py:37–39`; `app/ponto/services/veredito_celula.py:334–336`; `app/chamados/views_arquivar.py:77`) | C6 procura essas ações humanas posteriores à tranca. (`app/chamados/juizes.py:1016–1023`) |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` da pergunta | `veredito_lavrado.lavrar`, inclusive gravação em lote. (`app/chamados/veredito_lavrado.py:197–231`) | Crédito das perguntas no painel, usado na partição dos verbos. (`app/chamados/services/raiox.py:133–149`; `app/chamados/services/verbos.py:34–47`) |
| Verbo/papel da porta | É derivado pela partição; não é um campo de papel escrito pelo censo. (`app/chamados/services/verbos.py:30–52`) | Alimentação lê partição dos vivos; censo lê os verbos retornados. (`app/chamados/juizes.py:1228–1248`; `app/chamados/services/vigia_fio.py:642–645`) |

Os baldes que retiram dias do fio morto também têm fontes próprias:

| Insumo | Escritor/fonte → leitor |
|---

[truncado em 20000 de 23582 caracteres]

## 05/11 15:15 · sessao 98aa28e006c7 · - · - · origem=Ronald (tmux)

Achado de item 4: `test_a_unica_causa_e_a_sombra` passa com a cura CRON-15 revertida, mantendo o módulo auxiliar disponível e retirando as 15 aplicações do decorator. Ele só troca `DUR_MAX_S['sombra.sh']` por 4620 e chama `linhas()`/`render()`; nenhuma dessas operações lê os decorators ou executa os 15 testes. O teste, portanto, não prova a promessa de que os 15 passam após a duração caber. Referências: `app/core/tests/test_marca_cron15.py:67`, `app/core/tests/test_marca_cron15.py:69`, `app/core/tests/test_marca_cron15.py:70`, `app/core/tests/test_marca_cron15.py:71`, `app/config/crons.py:1785`, `app/config/crons.py:1865`, promessa em `app/core/tests/marca_cron15.py:9`. Toca dinheiro: não diretamente; efeito financeiro real `[nao verificado]`. Os outros testes de AST detectariam a retirada das aplicações; a fraqueza é desse selo individual (`app/core/tests/test_marca_cron15.py:48`).

Mapa dos valores novos:

| Valor | Escritor | Leitor |
|---|---|---|
| `PRAZO` | `app/core/tests/marca_cron15.py:24` | `app/core/tests/marca_cron15.py:54` |
| `ITEM_DE_FILA` | `app/core/tests/marca_cron15.py:25` | `app/core/tests/test_marca_cron15.py:89` |
| `CORREDOR_S` | `app/core/tests/marca_cron15.py:26` | `app/core/tests/test_marca_cron15.py:69` |
| `P95_QUE_CABE_S` | `app/core/tests/marca_cron15.py:27` | Nenhum leitor encontrado na cópia `[nao verificado]` fora dela |
| `MARCADOS` | `app/core/tests/marca_cron15.py:29` | `app/core/tests/test_marca_cron15.py:49`, `:50`, `:62` |
| `_HOJE_REAL` | `app/core/tests/marca_cron15.py:49`; mocks em `app/core/tests/test_marca_cron15.py:79`, `:84` | `app/core/tests/marca_cron15.py:54` |
| `__marca_cron15__` | `app/core/tests/marca_cron15.py:59` | `app/core/tests/test_marca_cron15.py:65` |
| Marca `expectedFailure` | `app/core/tests/marca_cron15.py:62` | Verificada em `app/core/tests/test_marca_cron15.py:80`, `:85`; consumo pelo runner `[nao verificado]` na cópia |

O decorator não consulta `MARCADOS`; a exclusividade é imposta pelo teste AST, cujo universo é a lista fixa `APPS` e arquivos imediatamente dentro de `<app>/tests`. Referências: `app/core/tests/marca_cron15.py:57`, `app/core/tests/test_marca_cron15.py:21`, `:28`, `:32`, `:48`.

O prazo é decidido uma vez durante o import/aplicação dos decorators; não é reavaliado durante a execução de cada teste. Referências: `app/core/tests/marca_cron15.py:49`, `:60`, `:62`. Isso coincide com a intenção expressa do próprio módulo (`app/core/tests/marca_cron15.py:17`); não trato como violação comprovada.

Não executei testes: há testes que criam/apagam arquivos temporários, incompatíveis com a restrição dada (`app/core/tests/test_marca_cron15.py:54`, `:56`; `app/chamados/tests/test_contract_crons.py:193`, `:194`, `:203`). Identidade do commit e primeiro pai `[nao verificado]`: a cópia não é um repositório Git.

## 05/11 15:15 · sessao 98611964b1c8 · - · - · origem=Ronald (tmux)

Lastro encontrado na cópia:

- `CLAUDE.md:194` e `app/docs/LEIS.md:96` exigem prazo, tripwire e item de fila para silenciamento.
- `app/docs/BACKLOG.md:747` registra especificamente os 15 nomes, prazo 13/11, sucesso inesperado como alarme de cura e proibição de cron/horário novo.
- `app/core/tests/marca_cron15.py:24` fixa o prazo; `:25` identifica a fila; `:60-62` aplica `expectedFailure` enquanto vigente.
- O aval CRON-15-E-RESTART consta em `app/core/tests/marca_cron15.py:1` e `app/docs/BACKLOG.md:747`; confirmação externa do aval: `[nao verificado]`.

Não encontrei regra de negócio alterada sem lastro demonstrável: o comportamento acrescentado está no controle de testes (`app/core/tests/marca_cron15.py:24-62`). O cadastro/data necessários a um contraexemplo de domínio não aparecem nos anexos fornecidos: `[nao verificado]`.

A referência ao vínculo histórico em `app/docs/JUIZES.md:98-101` identifica a autoridade existente, mas não comprova regressão introduzida por estas marcações (`app/core/tests/marca_cron15.py:29-46`).

Ponto para consolidação com o explorador de testes: `test_a_unica_causa_e_a_sombra` somente troca a duração e chama `linhas/render`; não executa os quinze testes (`app/core/tests/test_marca_cron15.py:67-71`). Logo seu nome não constitui prova de que todas as outras asserções passam com a sombra curada (`app/core/tests/test_marca_cron15.py:67-71`).

## 05/11 15:15 · sessao ed760dd5a920 · REVISAO-N1 · verificador · origem=Code

Há achado estático em N1: a instância compartilhada permite reutilizar uma tranca anterior à reabertura da competência (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/juizes.py:940`). Cadastro real atingido, ocorrência em produção e execução dos testes com reversão: **[nao verificado]**.

**1. Quem escreve e quem lê**

Os valores diretamente alterados por N1 são referências e caches em memória; o helper atribui o colaborador sem executar `save` (`app/chamados/juizes.py:985`).

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `disputa.colaborador`; também `celula.colaborador` no modo `disputas=True` | Helper atribui a instância encontrada pelo `colaborador_id` (`app/chamados/juizes.py:987`). | Tranca e classificação da pergunta (`app/chamados/juizes.py:930`, `app/chamados/juizes.py:1145`); censo das células (`app/chamados/services/vigia_fio.py:386`). |
| `ch.colaborador` compartilhado | Agrupamento de chamados do colaborador (`app/chamados/services/pergunta_no_app.py:136`). | `perguntas_do_app` e `retencao`, inclusive consulta da ata e de perguntas anteriores (`app/chamados/services/pergunta_no_app.py:139`, `app/chamados/services/pergunta_no_app.py:80`, `app/chamados/services/pergunta_no_app.py:94`). |
| Mapa `{colaborador_id: instância}` | Helper acrescenta a FK já carregada depois do `yield`; chamadores também semeiam o mapa (`app/chamados/juizes.py:991`; `app/chamados/services/pergunta_no_app.py:53`). | O próprio helper antes de entregar cada objeto (`app/chamados/juizes.py:988`). |
| `alim['instancias']` | `alimentar_portas`; complemento pelo censo e pela busca de `_no_app` (`app/chamados/juizes.py:1239`; `app/chamados/services/vigia_fio.py:581`; `app/chamados/juizes.py:1256`). | `_no_app` e alimentação de `perg_todas` (`app/chamados/juizes.py:1258`; `app/chamados/services/vigia_fio.py:586`). |
| `_tranca_memo`: empresa, período trancado e toque humano | `_memo_da_tranca` cria no request ou colaborador; leitores preenchem as entradas (`app/chamados/juizes.py:965`, `app/chamados/juizes.py:938`, `app/chamados/juizes.py:942`, `app/chamados/juizes.py:1025`). | `dia_da_pergunta_trancado` e `_admin_tocou_o_dia_depois` (`app/chamados/juizes.py:943`, `app/chamados/juizes.py:1010`). |
| `vivas`, `no_app`, `perg_todas` | Alimentação filtra pelo C6; `_no_app` guarda IDs; censo agrupa perguntas (`app/chamados/juizes.py:1242`, `app/chamados/juizes.py:1259`; `app/chamados/services/vigia_fio.py:588`). | Porta alcançável e classificação do dia sem porta (`app/chamados/juizes.py:1320`; `app/chamados/services/vigia_fio.py:609`, `app/chamados/services/vigia_fio.py:622`). |
| Limpeza de `_tranca_memo` no selo | Teste remove o atributo antes de cada medição (`app/ponto/tests/test_selo_chokepoint_escrita.py:417`). | As duas medições comparadas pelo teste (`app/ponto/tests/test_selo_chokepoint_escrita.py:426`). |

As decisões desses leitores alcançam escritas persistentes fora dos hunks do diff:

| Campos persistidos | Escritor alcançado | Leitor relevante |
|---|---|---|
| `materializada_em`, `materializada_por`, `via_resolucao` | `_escrever_carimbo`, chamado pelas portas de carimbo e descarimbo (`app/chamados/models.py:2532`, `app/chamados/models.py:2560`, `app/chamados/models.py:2616`). | C6 (`app/chamados/juizes.py:1087`, `app/chamados/juizes.py:1116`). |
| `respondida_em`, `validada_em`, `validada_por` | `carimbar`, opcionalmente; ressuscitação limpa os campos (`app/chamados/models.py:2561`, `app/chamados/models.py:1493`). | C6 e `respostas_sem_veredito` (`app/chamados/juizes.py:1078`, `app/chamados/juizes.py:1092`; `app/chamados/models.py:1772`). |
| `chamado_destino`, eventualmente `data_evento` | `reapontar_resolvedora` (`app/chamados/services/fio.py:831`, `app/chamados/services/fio.py:845`). | Migração compara motivo/data; C6 lê destino no julgamento do lastro (`app/chamados/services/fio.py:937`; `app/chamados/juizes.py:741`). |
| Via `duplicata`, carimbo e `recusa_motivo` | `retratar_duplicata` → `carimbar_identidade` (`app/chamados/services/fio.py:891`; `app/chamados/models.py:2604`). | C6 lê via/carimbo (`app/chamados/juizes.py:1111`). |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `veredito_lavrado.lavrar` (`app/chamados/veredito_lavrado.py:227`). | Verificação de validade temporal da lavra (`app/chamados/veredito_lavrado.py:179`). |
| `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | `DisputaSupervisao.fechar` (`app/chamados/models.py:1850`). | Porta de fechamento, reconciliador e relatório (`app/chamados/models.py:1809`; `app/chamados/reconciliador.py:634`; `app/chamados/relatorios_disputa.py:41`). |

A retratação e a resolução por chamado passam pelo C6 antes de carimbar; migração deduplica/reaponta; ressuscitação julga uma cópia sem os carimbos que pretende limpar (`app/chamados/reconciliador.py:610`, `app/chamados/reconciliador.py:620`; `app/chamados/services/disputa_emissao.py:1585`, `app/chamados/services/disputa_emissao.py:1603`; `app/chamados/services/fio.py:943`, `app/chamados/services/fio.py:947`; `app/chamados/models.py:1444`).

Fora do diff, `respostas_sem_veredito` também lê a tranca, `pode_fechar` combina essa lista com o fio, e a API consome `perguntas_do_app` (`app/chamados/models.py:1769`, `app/chamados/models.py:1783`; `app/api/views.py:2552`).

**2. Contraexemplo concreto**

Cadastro **hipotético** `colN1`, empresa E, corte 21; pergunta de **10/10/2026**, sem validação, materialização, célula ou toque humano; outubro inicialmente trancado. Existência desse cadastro em produção: **[nao verificado]**. Com esse corte, o dia pertence à janela 21/09–20/10 (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`).

Sequência fora de request:

1. `perguntas_do_app(c)` entrega a própria instância `c` ao C6. O memo fica nela, guarda o `PeriodoFechado`, e a pergunta sai como morta (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/juizes.py:965`, `app/chamados/juizes.py:942`, `app/chamados/juizes.py:1082`).
2. `reabrir_periodo`, recebendo outro objeto ORM desse período, grava `reaberto_em`. Uma consulta direta a `periodo_trancado` passa a devolver `None` (`app/ponto/services/fechamento.py:1407`, `app/ponto/services/fechamento.py:1438`).
3. Outra chamada a `perguntas_do_app(c)` consulta novamente as perguntas, mas reutiliza `c`. Como a chave já existe, não consulta novamente a tranca e mantém a pergunta fora da lista (`app/chamados/services/pergunta_no_app.py:46`, `app/chamados/services/pergunta_no_app.py:53`; `app/chamados/juizes.py:941`).
4. Sem essa tranca antiga, a pergunta sem materialização permanece viva (`app/chamados/juizes.py:1152`).

Isso diverge da reversibilidade da L-051 e da leitura da tranca prevista pelo corte C7; o próprio serviço declara que destrancar devolve a pergunta viva (`app/docs/LEIS.md:92`; `app/docs/CORTES.md:272`; `app/chamados/services/competencia_trancada.py:9`). É contraexemplo derivado do código; ocorrência desse encadeamento em produção: **[nao verificado]**.

**3. Regra de negócio alterada sem lastro**

No contraexemplo, a regra efetiva passa a ser **“vale a primeira tranca consultada durante a vida da instância compartilhada”**. N1 faz a instância do chamador atravessar chamadas, enquanto o memo consulta o juiz somente na ausência da chave (`app/chamados/services/pergunta_no_app.py:53`; `app/chamados/juizes.py:941`).

O lastro anexado manda ler a tranca e preservar a reversibilidade; autorização para manter a tranca antiga depois de reabrir: **[nao verificado]** (`app/docs/CORTES.md:272`; `app/docs/LEIS.md:92`).

Não há o mesmo contraexemplo para um novo toque humano: esse memo guarda somente o resultado positivo e volta a consultar os negativos (`app/chamados/juizes.py:1001`, `app/chamados/juizes.py:1024`).

**4. Selo que não morde**

`test_MORDE_duas_competencias_duas_contas` cria **uma pergunta por competência** e exige duas chamadas ao juiz (`app/chamados/tests/test_n1_perguntas_do_app.py:73`).

Revertendo a cura de `perguntas_do_app` conforme o anexo, cada pergunta consultaria sua competência uma vez: continuariam sendo duas chamadas. Portanto esse teste isolado passaria sem o compartilhamento; não exige repetição intracompetência (`app/chamados/tests/test_n1_perguntas_do_app.py:76`; `app/chamados/juizes.py:940`). Execução dessa reversão: **[nao verificado]**.

O teste de três perguntas da mesma competência exige uma chamada e cobre precisamente a reutilização que esse caso não cobre (`app/chamados/tests/test_n1_perguntas_do_app.py:58`).

**5. Cadastro de hoje decidindo passado**

| Campo | Escritor identificado | Leitura que decide passado |
|---|---|---|
| `situacao` | Desligamento e reativação (`app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:210`). | O contador admite apenas ativos atuais, embora classifique chamados anteriores a hoje (`app/chamados/services/pergunta_no_app.py:127`, `app/chamados/services/pergunta_no_app.py:73`). |
| `data_demissao` | Desligamento grava; reativação limpa (`app/colaboradores/services/desligamento.py:105`, `app/colaboradores/services/desligamento.py:211`). | Grade usa a data atual para classificar dias da janela; export exclui o fechamento solicitado sempre que ela não é nula (`app/escala/services/leitor_celula.py:366`; `app/folha/export.py:746`, `app/folha/export.py:840`). |
| `posto` | Transferência (`app/colaboradores/views.py:1385`, `app/colaboradores/views.py:1388`). | Calendário busca feriados do período pelo município do posto atual (`app/colaboradores/services/calendario.py:240`; `app/core/feriados.py:113`). |
| `empresa` | Transferência acompanha a empresa do novo posto (`app/colaboradores/views.py:1386`). | Tranca de `data_evento` e competência do chamado usam a empresa do cadastro entregue (`app/chamados/juizes.py:929`, `app/chamados/juizes.py:936`, `app/chamados/juizes.py:1996`). |
| Regime da empresa | Edição da empresa → porta da estrutura (`app/colaboradores/views.py:512`, `app/colaboradores/views.py:523`; `app/colaboradores/services/estrutura.py:17`). | A régua recebe competência, mas escolhe CLT/CCT pelo regime atual da empresa (`app/core/regua_cct.py:297`, `app/core/regua_cct.py:352`). |
| `isento_ponto` | Passe livre → porta do colaborador (`app/colaboradores/views.py:176`; `app/colaboradores/services/colaborador.py:47`). | Flag atual sobrepõe a grade da janela e faz `eventos_do_fechamento` devolver lista vazia (`app/escala/services/leitor_celula.py:349`, `app/escala/services/leitor_celula.py:412`; `app/folha/export.py:406`). |
| Vínculo atual | Serviço atualiza/reativa ou cria vínculo (`app/colaboradores/services/vinculo.py:265`, `app/colaboradores/services/vinculo.py:278`). | A consulta inicial ao ativo existe, mas a precedência completa geradoras faltantes e consulta o vínculo do dia. Nesse caminho, não ficou demonstrada substituição indevida pelo vínculo atual (`app/colaboradores/services/calendario.py:235`; `app/ponto/precedencia.py:327`, `app/ponto/precedencia.py:351`; `app/docs/JUIZES.md:98`). |

Essas leituras demonstram dependência do cadastro atual. Elas, isoladamente, não provam infração normativa nem mudança causada por N1; exigência de empresa/posto/regime/isenção históricos para cada caminho, nos anexos fornecidos: **[nao verificado]**. O efeito financeiro de emissão e régua existe no código; diferença efetivamente produzida em dinheiro: **[nao verificado]** (`app/folha/export.py:408`, `app/folha/export.py:840`; `app/core/regua_cct.py:354`).

**VEREDITO: ACHADO**

1. `app/chamados/services/pergunta_no_app.py:53`; `app/chamados/juizes.py:941` — **itens 1, 2 e 3**: tranca antiga reutilizada após reabertura, no contraexemplo estático. **Toca dinheiro: nao**, no caminho demonstrado.
2. `app/chamados/tests/test_n1_perguntas_do_app.py:73` — **item 4**: selo isolado passa com a cura revertida. **Toca dinheiro: nao**.
3. `app/chamados/juizes.py:936`; `app/chamados/services/pergunta_no_app.py:127` — **item 5**: empresa/situação atuais participam do julgamento do passado. **Toca dinheiro: nao**, nesses leitores.
4. `app/folha/export.py:406`; `app/folha/export.py:840`; `app/core/regua_cct.py:352` — **item 5**: isenção, demissão e regime atuais decidem eventos, inclusão ou régua de competência solicitada. Regressão N1 **[nao verificado]**. **Toca dinheiro: sim**.

## 05/11 15:15 · sessao ba7dd5d0a0f4 · - · - · origem=Ronald (tmux)

Levantamento somente leitura, sem execução de commands de negócio.

- `DUR_MAX_S`: escritor da configuração é `app/config/crons.py:159-160`, mesclando `_DUR_DECLARADA` com o JSON carregado em `app/config/crons.py:146-156`. O JSON é escrito pelo medidor em `bin/crons_duracao.py:76` e `bin/crons_duracao.py:93-99`; sua fonte é o placar produzido pelo envelope em `bin/cron_run.sh:13`.
- A duração é lida por `inicio_derivado` em `app/config/crons.py:241-259`; a sombra chama essa função ao ler `sched`, pela entrada em `app/config/crons.py:331` e pelo acesso adiado em `app/config/crons.py:92-99`. `linhas` consome `sched` em `app/config/crons.py:1791`; `render` chama `linhas` em `app/config/crons.py:1865-1866`.
- A cópia contém sombra de **5188 s** (`app/config/crons_duracao.json:78`), vizinho **05:25** (`app/config/crons.py:219`), tolerância **120 s** (`app/config/crons.py:183`) e piso **04:05** (`app/config/crons.py:182`). Pelas operações em `app/config/crons.py:248-258`, a duração arredonda para 87 minutos e exige início **03:56**, provocando o `ValueError`.
- `corrida_no_horario` lê a agenda vigente, por nome e empresas, em `app/config/crons.py:2220-2240`; o medidor usa esse filtro ao ler as corridas em `bin/crons_duracao.py:73-75`. O teste decorado enumera `sched` de todos os crons antes de testar cron livre/desconhecido, em `app/core/tests/test_contract_crons_duracao_horario.py:54-57`, atingindo também a sombra.
- O placar lê a agenda de `CRONS` em `app/core/management/commands/pipeline_placar.py:38-50`, lê as corridas do dia solicitado em `app/core/management/commands/pipeline_placar.py:60-80` e classifica o resultado em `app/core/management/commands/pipeline_placar.py:84-109`. O comando aceita `--dia` passado em `app/core/management/commands/pipeline_placar.py:120-123`. Portanto há agenda atual julgando execução cron passada, mas isso já existe fora do diff e não é cadastro de colaborador; divergência contra regra de histórico de agenda: `[nao verificado]`.
- Os trechos desse fluxo não leem `situacao`, `data_demissao`, posto, empresa cadastral, regime, `isento_ponto` nem vínculo atual para decidir dia/competência de colaborador: eles trabalham com configuração de cron e corridas (`app/config/crons.py:241-259`, `app/config/crons.py:1785-1815`, `app/core/management/commands/pipeline_placar.py:38-109`). O diff fornecido altera somente módulos de teste/decoradores; mudança de regra de negócio ou efeito financeiro atribuível a ele: `[nao verificado]`.
- Não há cadastro raspado individual no material fornecido. Cadastro/data concretos de produção para os itens 2 e 5: `[nao verificado]`.

## 05/11 15:15 · sessao 38300e9c5e3b · REVISAO-BUG-SW · verificador · origem=Code

1. **Quem escreve e quem lê.** A mudança é no valor entregue a `respondWith`; não altera campo de cadastro (`app/static/sw.js:29`, `app/static/sw.js:48`, `app/static/sw.js:77`, `app/static/sw.js:84`).

   | Valor | Quem escreve/produz | Quem lê/consome |
   |---|---|---|
   | Resposta da rede | `fetch` nos três caminhos: `app/static/sw.js:48`, `:71`, `:84` | `respondWith`: `app/static/sw.js:47`, `:70`, `:83` |
   | Resposta guardada | Instalação com `addAll`: `app/static/sw.js:12`; gravações com `put`: `:59`, `:74`; exclusão de caches antigos: `:20` | Novo helper com `caches.match`: `app/static/sw.js:30`; leitor estático preexistente: `:56` |
   | Erro quando falta cópia | `Response.error()`: `app/static/sw.js:30` | Chamadores do helper: `app/static/sw.js:48`, `:77`, `:84` |
   | Identificador do cache, afetado pelo conteúdo do SW | Hash inclui `sw.js`: `app/core/views.py:524`, `:548`, `:552`; substituição de `__BUILD_ID__`: `:572` | Nome em `app/static/sw.js:1`, usado na abertura e limpeza: `:12`, `:20`, `:59`, `:74` |
   | Resultados do selo | Rede/cache simulados: `app/core/tests/test_sw_offline_fallback.py:33`, `:37`, `:39`; captura de resposta, tipo e corpo: `:45`, `:49` | Asserções: `app/core/tests/test_sw_offline_fallback.py:83`, `:87`, `:94`, `:101` |

2. **Contraexemplo de cadastro e data contra LEIS/CORTES:** não demonstrado. O anexo `BUG-SW.leis` veio vazio e o dado fornecido não contém cadastro nem data de apuração concretos. **[nao verificado]**

   O cenário técnico demonstrável é GET `/escala/?app=1`, navegação sem rede e sem cache: o teste fornece essas condições e exige uma `Response` de tipo `error`; o helper implementa esse retorno (`app/core/tests/test_sw_offline_fallback.py:63`, `:80`, `:84`, `:87`; `app/static/sw.js:30`). Isso não demonstra divergência de regra de negócio para uma pessoa/data. **[nao verificado]**

3. **Regra de negócio alterada sem lastro:** não identificada nos caminhos alterados. A alteração acrescenta a resposta de erro ao fallback; mantém rede primeiro e devolução da cópia quando existente (`app/static/sw.js:30`, `:48`, `:71`, `:77`, `:84`). `JUIZES.md` declara ser tabela de consulta, não a própria lei (`app/docs/JUIZES.md:7`). Lastro específico nas LEIS/CORTES anexas: **[nao verificado]**.

4. **Selo que não morde:** há dois testes individuais que passariam com as três chamadas da cura revertidas, por análise dos caminhos: [nao verificado]

   - `test_MORDE_sem_rede_COM_cache_a_copia_continua_vencendo`: fornece uma cópia e exige `DO-CACHE`; devolver diretamente `caches.match` já satisfaz essa condição (`app/core/tests/test_sw_offline_fallback.py:33`, `:37`, `:89`, `:94`).
   - `test_MORDE_com_rede_a_rede_vence_o_cache`: fornece rede funcionando; o fallback alterado sequer é necessário para obter `DA-REDE` (`app/core/tests/test_sw_offline_fallback.py:39`, `:96`, `:101`; `app/static/sw.js:48`, `:71`, `:84`).

   **A suíte completa morde a reversão:** o `RED` exige uma `Response` de tipo `error` justamente sem rede e sem cópia (`app/core/tests/test_sw_offline_fallback.py:79`, `:84`, `:87`). Execução dos testes nesta revisão: **[nao verificado]**; Node não foi localizado no PATH.

5. **Cadastro atual decidindo passado:** não identificado no fluxo alterado. O roteamento consulta método, URL e modo da requisição; o helper consulta o cache e retorna resposta/erro, sem interpretar o corpo nem consultar `situacao`, `data_demissao`, `posto`, `empresa`, `regime`, `isento_ponto` ou vínculo atual (`app/static/sw.js:29`, `:34`, `:36`, `:41`, `:46`, `:54`, `:69`). O servidor que entrega o SW lê arquivos e calcula seu identificador (`app/core/views.py:548`, `:570`). Efeito sobre cadastro ou competência passada concreta em produção: **[nao verificado]**.

VEREDITO: ACHADO

1. `app/core/tests/test_sw_offline_fallback.py:89` — item 4: teste individual passa com a cura revertida; toca dinheiro: **nao**, verifica corpo da resposta (`:94`).
2. `app/core/tests/test_sw_offline_fallback.py:96` — item 4: teste individual passa com a cura revertida; toca dinheiro: **nao**, verifica corpo da resposta (`:101`).

## 05/11 15:15 · sessao bc7241f3d2dc · - · - · origem=Ronald (tmux)

Auditoria estática do mapa, somente leitura:

- `_celulas_do_ato` é novo estado transitório, escrito em `app/escala/alimentacao.py:44`, restaurado/removido em `app/escala/alimentacao.py:49` e `app/escala/alimentacao.py:51`; o único leitor direto é `app/escala/models.py:1063`. A janela limita seu uso em `app/escala/models.py:1067`.
- Os quatro leitores adotam o mapa apenas quando `celulas is None`: trabalho (`app/escala/models.py:1087`), marcos (`app/escala/models.py:1117`), pausas (`app/escala/models.py:1182`) e extra (`app/escala/models.py:1227`). Mapa explícito continua vencendo; os retornos e os fallbacks anteriores permanecem nos mesmos métodos (`app/escala/models.py:1094`, `app/escala/models.py:1124`, `app/escala/models.py:1189`, `app/escala/models.py:1234`).
- Chamadores de produção de `apurar_por_fatia`: fechamento (`app/ponto/services/fechamento.py:736`) e autoridade do espelho (`app/ponto/services/espelho.py:1164`). Ambos carregam janelas completas: envelope por cortes em `app/ponto/services/fechamento.py:575` e consulta em `app/ponto/services/fechamento.py:591`; janela exata do espelho em `app/ponto/services/espelho.py:1049`.
- `trabalha`, `dna` e `escala_geradora` são persistidos pela porta em `app/ponto/portas/celula.py:247` e `app/ponto/portas/celula.py:260`. `montar_dna` congela marcos, pausas e extra do template em `app/ponto/management/commands/gerar_celulas.py:81`, `app/ponto/management/commands/gerar_celulas.py:82` e `app/ponto/management/commands/gerar_celulas.py:122`.
- Consumidores efetivamente afetados: cursor de previstos do escritor (`app/ponto/services/fechamento.py:713`) e marcos implícitos do motor em janela HE (`app/ponto/motor_calculo_v2.py:1524`), pontualidade (`app/ponto/motor_calculo_v2.py:2041`), blocos múltiplos (`app/ponto/motor_calculo_v2.py:2689`), Zona 5 (`app/ponto/motor_calculo_v2.py:3021`) e limite diário (`app/ponto/motor_calculo_v2.py:3360`). Extra e trabalho do motor já recebem mapa explícito (`app/ponto/motor_calculo_v2.py:1347`, `app/ponto/motor_calculo_v2.py:2436`).
- Valores locais deslocados pelo diff: `motor`, impressão/fonte da régua, mapa de régua por dia e resultado da fatia são escritos em `app/ponto/services/fechamento.py:270`, `app/ponto/services/fechamento.py:272`, `app/ponto/services/fechamento.py:274`, `app/ponto/services/fechamento.py:276` e `app/ponto/services/fechamento.py:277`; o resultado é composto em `app/ponto/services/fechamento.py:285`, a régua por dia/ato é consumida pelo lavrador em `app/ponto/services/fechamento.py:1205`, e os trechos de régua são lidos pelo espelho em `app/ponto/services/espelho.py:1166` e `app/ponto/services/espelho.py:1296`.

Não demonstrei regra de negócio alterada pelo mapa isolado: a substituição preserva leitura da célula congelada, compatível com `app/docs/JUIZES.md:22`; ocorrência de resultado monetário diferente causado pelo diff em produção é [nao verificado].

**Item 4 confirmado por inspeção:** `test_a_apuracao_do_motor_e_a_da_REGRA_pela_celula` afirma números, mas passaria com a integração do mapa revertida: sem contexto, o mesmo DNA seria lido do banco (`app/ponto/tests/test_he_pendente_identity_map.py:149`; `app/escala/models.py:1122`). O teste do retrato também não prova essa cura (`app/ponto/tests/test_he_pendente_identity_map.py:109`; o próprio comentário declara a independência em `app/ponto/tests/test_he_pendente_identity_map.py:20`). O mutante fornece mapa vazio, não reverte a cura (`app/ponto/tests/test_he_pendente_identity_map.py:161`). Há, entretanto, um selo específico que deve distinguir a reversão: contador de consultas (`app/ponto/tests/test_he_pendente_identity_map.py:116`). Execução dos testes e resultado efetivo: [nao verificado].

## 05/11 15:14 · sessao 796bd0f560a7 · - · - · origem=Ronald (tmux)

Item 5: há leituras do cadastro atual decidindo passado, anteriores ao diff; vítimas reais e valores de produção correspondentes são [nao verificado].

| Campo | Escritor encontrado | Leitor que decide passado |
|---|---|---|
| `situacao`, `data_demissao` | `app/colaboradores/services/desligamento.py:104`; reativação apaga demissão em `:210` | `app/ponto/services/he_pendente_lavrado.py:92` inclui somente `ativo` atual, apesar de apurar janela por mês/ano em `:89`. O universo do fechamento inclui desligados conforme a data em `app/ponto/services/fechamento.py:423` e `:430`. |
| `posto`, `empresa` | Transferência grava ambos em `app/colaboradores/views.py:1385`, salva em `:1388` e registra histórico datado em `:1392`. Edição também escreve empresa em `:1313`, salva em `:1342`. | Feriados usam município do posto atual em `app/core/feriados.py:106`, `:113`; fechamento injeta esses feriados em `app/ponto/services/fechamento.py:628` e `:720`. Espelho faz o mesmo em `app/ponto/services/espelho.py:139`, `:1030` e `:1113`. |
| Empresa atual / corte | Mesmos escritores acima. | A janela passada é resolvida pela empresa atual em `app/ponto/services/fechamento.py:607`; o filtro por empresa atual fica em `:435` e `app/ponto/services/he_pendente_lavrado.py:92`. |
| `regime_trabalhista` | Formulário: `app/colaboradores/views.py:512`, `:521`; persistência: `app/colaboradores/services/estrutura.py:17`. | Para uma `competencia` recebida, a régua consulta vínculo histórico em `app/core/regua_cct.py:329`, mas lê empresa/regime atuais em `:352` e `:353`; decide CLT/CCT em `:354` e `:366`. O juiz documentado é `app/docs/JUIZES.md:30`. |
| `isento_ponto` | `app/colaboradores/views.py:173`, `:176`; porta persiste em `app/colaboradores/services/colaborador.py:47`. | Juiz sem argumento de data: `app/core/constants.py:13`. Elimina furos de qualquer período em `app/ponto/services/fechamento.py:357`; zera emissão de eventos do fechamento em `app/folha/export.py:407`. O comentário reconhece snapshot anterior à isenção em `:402`. |
| Vínculo | Fechamento de vigência: `app/colaboradores/services/vinculo.py:561`; encerramento: `:797`; trecho retroativo: `:832`. | O caminho principal usa janela/geradora, não simplesmente o vínculo atual: `app/ponto/services/fechamento.py:639`, `:191`; CCT em `app/core/regua_cct.py:329`; autoridade em `app/docs/JUIZES.md:98`. Resumo de cabeçalho usa ativo atual em `app/ponto/services/espelho.py:208`, mas é apresentação em `:240`. |

Contraexemplo sintético, derivado do cadastro do próprio teste: os três dias 01/10, 02/10 e 05/10 têm batidas e DNA 08–17 em `app/ponto/tests/test_he_pendente_identity_map.py:33`, `:84`, `:94`; o teste espera três pontas de dez minutos em `:109`. Se esse colaborador for desligado em 21/10/2026 e a competência 10 for relida em 05/11/2026, o filtro atual o elimina integralmente em `app/ponto/services/he_pendente_lavrado.py:92`, embora a seleção temporal do fechamento o mantenha em `app/ponto/services/fechamento.py:423` e `:430`. Execução desse contrafactual e ocorrência real: [nao verificado].

Não atribuir esses problemas à introdução do Identity Map: as leituras ficam fora das linhas alteradas no HE.patch fornecido. A existência de autorização específica nos LEIS/CORTES anexados para cada efeito retrospectivo de posto, regime e isenção é [nao verificado]; o juiz histórico de vínculo está explicitamente documentado em `app/docs/JUIZES.md:98`.

## 05/11 15:14 · sessao 592753a355bf · - · - · origem=Ronald (tmux)

Prova estática; execução dos testes e dados reais de produção [nao verificado]. [nao verificado]

- **Selo que não morde reversão:** `test_o_retrato_e_o_da_REGRA_pela_celula` apenas verifica 10 minutos nos dias 01, 02 e 05/10/2026; a própria descrição reconhece que o retrato nasce fora de `apurar_por_fatia`. Com a cura revertida, o leitor consulta o mesmo DNA no banco. Referências: `app/ponto/tests/test_he_pendente_identity_map.py:109`, `app/ponto/tests/test_he_pendente_identity_map.py:21`, `app/escala/models.py:1122`.
- **Também passa com a cura revertida:** `test_a_apuracao_do_motor_e_a_da_REGRA_pela_celula` verifica resultado financeiro preservado pela otimização, 480 trabalhados/10 fora por dia; sem mapa, o DNA continua sendo lido do banco. Referências: `app/ponto/tests/test_he_pendente_identity_map.py:149`, `app/escala/models.py:1122`, `app/escala/models.py:1124`.
- **O MORDE testa outro defeito:** substitui o mapa completo por `{}`, produzindo ausência fictícia da célula e fallback ao template; reversão da cura consulta a célula existente no banco. Portanto, reprovar esse mutante não demonstra reprovar reversão. Referências: `app/ponto/tests/test_he_pendente_identity_map.py:161`, `app/ponto/tests/test_he_pendente_identity_map.py:168`, `app/escala/models.py:1133`.
- **A suíte contém um selo específico da cura:** o RED de queries exige zero consultas de célula feitas por `marcos_do_dia`/`eh_dia_trabalho` dentro de `apurar_por_fatia`; ele deve detectar a reversão pelo custo, embora a execução esteja [nao verificado]. Referências: `app/ponto/tests/test_he_pendente_identity_map.py:47`, `app/ponto/tests/test_he_pendente_identity_map.py:116`.

Escritores e leitores do insumo:

| Valor | Escritores | Leitores |
|---|---|---|
| DNA: marcos, pausas, extra | `montar_dna` obtém marcos/pausas do template e congela os valores; extra só entra quando há número e o dia trabalha. `app/ponto/management/commands/gerar_celulas.py:73`, `:81`, `:92`, `:119` | `marcos_do_dia`, `pausas_do_dia`, `extra_declarada_do_dia`: `app/escala/models.py:1124`, `:1189`, `:1235` |
| `trabalha`, `escala_geradora`, `dna` persistidos | Regeneração cria/altera: `app/ponto/portas/celula.py:243`, `:247`, `:260`. Geração periódica cria/altera: `:590`, `:593`, `:612`. Reversão restaura: `:530`, `:532`, `:535` | `eh_dia_trabalho`: `app/escala/models.py:1094`; seleção do vínculo: `app/escala/alimentacao.py:269` |
| Marcos consumidos pelo motor | Função capturada do vínculo em `app/ponto/motor_calculo_v2.py:2278` | Janela de HE chama sem `celulas=` em `:1524`; pontualidade chama em `:2041` |
| Extra consumida | Função capturada em `app/ponto/motor_calculo_v2.py:2284` | Espelho consulta explicitamente a célula completa em `app/ponto/services/espelho.py:733`, depois marca as pontas em `:734` |

Não localizei mudança nova de regra de negócio no diff sob seu contrato de mapa completo: os mesmos ramos preservam `trabalha`, DNA, ausência de pausa/extra e fallback ao template. Referências: `app/escala/models.py:1094`, `:1124`, `:1202`, `:1234`.

A regra citada no teste, L-110, exige referência pela lei, casos antes do código e propriedades fixas; não exige que uma otimização semanticamente equivalente altere o resultado financeiro. Referência: `app/docs/LEIS.md:136`. A obrigação de leitores consumirem a lavratura consta na L-095, com implementação reconhecida como parcial; a L-113 revoga a trava de exportada e preserva trilha/TXT/DIFF. Referências: `app/docs/LEIS.md:113`, `app/docs/LEIS.md:149`.

_1577 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
