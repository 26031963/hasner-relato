# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 02:05** · publicado em **15/11/2026 02:06** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 15/11 01:42 · sessao bfb6a09b5bab · BATIDA-DEDUP-RETRATADA · sonda · origem=Code

```python
# Django shell; tenant juliani ja ativo. Producao: [nao verificado].
# Guardas e UTC-03 conforme q3a fornecida: [nao verificado].
# Convencoes desta sonda: "-" = inaplicavel; "[nao verificado]" = desconhecido.
# FALHA_T4 informa a primeira condicao que falharia na q3a fornecida.
# Ausencia/divergencia diagnosticada nao incrementa ERRO; falha de leitura sim.
# Correspondencia e resultados atuais em producao: [nao verificado].

erros = 0
nivel_logging_anterior = None
pergunta_atual = None


def erro(etapa, exc):
    global erros
    erros += 1
    alvo = str(pergunta_atual) if pergunta_atual is not None else "SONDA"
    print(
        "P{}_ERRO_{} {}".format(alvo, etapa, type(exc).__name__),
        flush=True,
    )


try:
    import logging
    import re
    from contextlib import ExitStack
    from datetime import timedelta, timezone as dt_timezone
    from unittest.mock import patch

    from django.db import connection, connections
    from django.utils import timezone
    from django.utils.dateparse import parse_datetime

    nivel_logging_anterior = logging.root.manager.disable
    logging.disable(logging.CRITICAL)

    alias_leitura = connection.alias
    local = dt_timezone(timedelta(hours=-3))

    def somente_select(execute, sql, params, many, context):
        if (
            many
            or context["connection"].alias != alias_leitura
            or not re.match(r"\s*SELECT\b", sql, re.I)
            or ";" in sql
            or re.search(
                r"\bFOR\s+(?:(?:NO\s+KEY|KEY)\s+)?(?:UPDATE|SHARE)\b",
                sql,
                re.I,
            )
        ):
            raise RuntimeError("consulta_recusada")
        return execute(sql, params, many, context)

    # app/core/observ.py:73; app/chamados/juizes.py:1473 e :1486.
    def best_effort_falhou(logger_nome, operacao, exc, **contexto):
        erro("BEST_EFFORT", exc)

    with ExitStack() as pilha:
        for alias in connections:
            pilha.enter_context(
                connections[alias].execute_wrapper(somente_select)
            )
        pilha.enter_context(timezone.override(local))
        pilha.enter_context(
            patch("core.observ.registrar_engolido", new=best_effort_falhou)
        )

        from chamados.models import PerguntaDisputa
        from chamados.juizes import tranca_da_consulta
        from ponto.models import Batida
        from ponto.janelas import janela_atual
        from folha.export import competencia_entregue

        with connection.cursor() as cursor:
            cursor.execute("SELECT current_schema()")
            if cursor.fetchone()[0] != "juliani":
                raise RuntimeError("tenant_incorreto")

        cache_exportada = {}

        def competencia_no_dia(empresa, dia):
            # app/ponto/janelas.py:60; app/folha/export.py:556 e :588.
            _, fim = janela_atual(dia, empresa)
            chave = (empresa.pk, fim.month, fim.year)
            if chave not in cache_exportada:
                cache_exportada[chave] = competencia_entregue(*chave)
            return (
                "{:04d}-{:02d}".format(fim.year, fim.month),
                int(cache_exportada[chave]),
            )

        def sondar(pk, dados):
            # Consulta direta aos tres PKs, sem excluir perguntas agora mortas.
            # Vida exata: app/chamados/models.py:3474.
            try:
                p = (
                    PerguntaDisputa.objects.using(alias_leitura)
                    .select_related(
                        "disputa__colaborador__empresa",
                        "celula",
                        "chamado_destino",
                        "batida_origem",
                    )
                    .defer(
                        "disputa__colaborador__nome",
                        "disputa__colaborador__cpf",
                    )
                    .get(pk=pk)
                )
            except PerguntaDisputa.DoesNotExist as exc:
                dados["PERGUNTA_EXISTE"] = 0
                erro("PERGUNTA", exc)
                return

            dados["PERGUNTA_EXISTE"] = 1

            # app/chamados/models.py:2494, :2511 e :1783.
            dados["COLABORADOR_DISPUTA_PK"] = p.disputa.colaborador_id
            dados["COLABORADOR_PERGUNTA_PK"] = p.colaborador_id

            # app/colaboradores/models.py:321.
            empresa = p.disputa.colaborador.empresa
            dados["EMPRESA_Q3A_PK"] = empresa.pk

            ub = None
            bid = None
            dia_contexto = None
            dia_batida = None

            try:
                # Contexto: app/chamados/models.py:1792.
                # Formato efetivo destes registros: [nao verificado].
                ctx = p.disputa.contexto
                if ctx is None:
                    ctx = {}
                if not isinstance(ctx, dict):
                    raise ValueError("contexto_invalido")

                ub = ctx.get("ultima_batida")
                if not isinstance(ub, dict):
                    raise ValueError("ultima_batida_ausente_ou_invalida")

                bruto_id = ub.get("id")
                if (
                    isinstance(bruto_id, bool)
                    or not isinstance(bruto_id, (int, str))
                    or not re.fullmatch(r"[0-9]+", str(bruto_id))
                    or int(bruto_id) <= 0
                ):
                    raise ValueError("batida_id_invalido")

                bid = int(bruto_id)
                dados["ULTIMA_BATIDA_ID"] = bid
            except BaseException as exc:
                erro("CONTEXTO_ID", exc)

            if isinstance(ub, dict):
                try:
                    bruto_ts = ub.get("timestamp")
                    if not isinstance(bruto_ts, str):
                        raise ValueError("timestamp_ausente_ou_invalido")

                    ts = parse_datetime(bruto_ts)
                    if ts is None or timezone.is_naive(ts):
                        raise ValueError("timestamp_invalido_ou_sem_fuso")

                    dia_contexto = ts.astimezone(local).date()
                    dados["DIA_CONTEXTO_LOCAL"] = dia_contexto.isoformat()
                except BaseException as exc:
                    erro("CONTEXTO_DIA", exc)

            if bid is not None:
                try:
                    # app/ponto/models.py:22, :23 e :67.
                    b = (
                        Batida.objects.using(alias_leitura)
                        .filter(pk=bid)
                        .values(
                            "pk", "colaborador_id", "timestamp", "retratada_em"
                        )
                        .first()
                    )
                    dados["BATIDA_EXISTE"] = int(b is not None)

                    if b is None:
                        dados["FALHA_T4"] = "batida_referenciada_ausente"
                        for chave in (
                            "BATIDA_RETRATADA",
                            "COLABORADOR_BATIDA_PK",
                            "DIA_BATIDA_LOCAL",
                            "COMPETENCIA_BATIDA",
                            "EXPORTADA_BATIDA",
                        ):
                            dados[chave] = "-"
                    else:
                        dados["BATIDA_RETRATADA"] = int(
                            b["retratada_em"] is not None
                        )
                        dados["COLABORADOR_BATIDA_PK"] = b["colaborador_id"]

                        cid = p.disputa.colaborador_id
                        dados["FALHA_T4"] = (
                            "colaborador_divergente"
                            if (
                                b["colaborador_id"] != cid
                                or p.colaborador_id != cid
                            )
                            else "nenhuma_das_duas"
                        )

                        if timezone.is_naive(b["timestamp"]):
                            raise ValueError("batida_timestamp_sem_fuso")

                        dia_batida = b["timestamp"].astimezone(local).date()
                        dados["DIA_BATIDA_LOCAL"] = dia_batida.isoformat()
                except BaseException as exc:
                    erro("BATIDA", exc)

            try:
                # app/chamados/juizes.py:1128;
                # app/chamados/models.py:3484.
                antes = erros
                with tranca_da_consulta([p]):
                    viva = int(p.viva)

                if erros == antes:
                    dados["VIVA"] = viva
            except BaseException as exc:
                erro("VIDA", exc)

            # Usar a empresa da disputa para ambos os dias, conforme q3a
            # fornecida. FK: app/colaboradores/models.py:321.
            # Consultar exportacao mesmo se houver divergencia ou pergunta morta.
            for rotulo, dia in (
                ("CONTEXTO", dia_contexto),
                ("BATIDA", dia_batida),
            ):
                if dia is None:
                    continue
                try:
                    comp, exportada = competencia_no_dia(empresa, dia)
                    dados["COMPETENCIA_" + rotulo] = comp
                    dados["EXPORTADA_" + rotulo] = exportada
                except BaseException as exc:
                    erro("EXPORTACAO_" + rotulo, exc)

        campos = (
            "PERGUNTA_EXISTE",
            "ULTIMA_BATIDA_ID",
            "FALHA_T4",
            "BATIDA_EXISTE",
            "BATIDA_RETRATADA",
            "COLABORADOR_DISPUTA_PK",
            "COLABORADOR_PERGUNTA_PK",
            "COLABORADOR_BATIDA_PK",
            "DIA_CONTEXTO_LOCAL",
            "DIA_BATIDA_LOCAL",
            "VIVA",
            "EMPRESA_Q3A_PK",
            "COMPETENCIA_CONTEXTO",
            "EXPORTADA_CONTEXTO",
            "COMPETENCIA_BATIDA",
            "EXPORTADA_BATIDA",
        )

        for pk in (115, 116, 1339):
            pergunta_atual = pk
            dados = dict.fromkeys(campos, "[nao verificado]")
            try:
                sondar(pk, dados)
            except BaseException as exc:
                erro("ITEM", exc)
            finally:
                for chave in campos:
                    print(
                        "P{}_{} {}".format(pk, chave, dados[chave]),
                        flush=True,
                    )

        pergunta_atual = None

except BaseException as exc:
    erro("SONDA", exc)
finally:
    try:
        if nivel_logging_anterior is not None:
            logging.disable(nivel_logging_anterior)
    except BaseException as exc:
        erro("RESTAURA_LOGGING", exc)

    print("ERRO {}".format(erros), flush=True)
    print("FIM ok={}".format(int(erros == 0)), flush=True)
```

## 15/11 01:35 · sessao 0f4c61a1ae54 · SILENCIO-VENCIDO · sonda · origem=Code

```python
"""
SONDA SILENCIO-VENCIDO. Execucao na SOMBRA: [nao verificado].

FechamentoMensal esta em app/ponto/models.py:398; o arquivo pedido,
app/folha/models.py:310, referencia esse modelo. Os campos solicitados
sao totais mensais, sem decomposicao diaria:
app/ponto/models.py:405, :408, :412, :432, :440, :445.
Por isso, DIA.gravado=null; gravado_competencia informa o total mensal.

Lavra diaria: mesma chamada da m2 colada [nao verificado por arquivo local],
linhas_do_periodo(colab, ini, fim), ds.get(dia), getattr(dp, campo).
Contrato: app/ponto/services/dia_pago.py:833 e :855.
Soma mensal: app/ponto/services/dia_pago.py:550.

InsumosDoDono.dono(data) carrega os insumos e chama dono_da_divergencia:
app/ponto/management/commands/e6_oraculo.py:182, :190, :195.
Preparo usado pelo command: mesmo arquivo:316, :328, :378.

Sem gravado diario, percorremos TODOS os dias de janela_fechamento:
app/ponto/janelas.py:54. O dono e diario; sua responsabilidade pela
diferenca mensal continua [nao verificado]. Nao elegemos dono mensal.

TOT unidade=dia_campo: empresa x campo individual x dono; conta DIA.
As incidencias de pares podem se sobrepor entre campos e donos.
TOT unidade=par: empresa x conjunto exato de campos x conjunto de donos
encontrados; cada par completo entra uma vez, fechando 59.
O conjunto de donos e um perfil, nao um novo veredito mensal.
59 pares e 65 ocorrencias de campos: [nao verificado — anexo colado].

FIM ok=1 indica coleta completa sem excecao, nao divergencia resolvida.
Bloqueios baseados no anexo m2_sonda.py [nao verificado por arquivo local]
e em app/chamados/management/commands/diff_e3_sombra.py:30.
"""
import sys

_BYTECODE_ANTERIOR = sys.dont_write_bytecode
sys.dont_write_bytecode = True

import datetime as dt
import json
import logging
import re
from collections import Counter, defaultdict
from contextlib import ExitStack, redirect_stdout, redirect_stderr
from decimal import Decimal, localcontext
from unittest.mock import patch

# Relogio da m2 colada: [nao verificado por arquivo local].
AGORA = dt.datetime.fromisoformat("2026-11-15T02:20:00+00:00")

R = "minutos_realizados"
A = "minutos_abonados"
P = "minutos_previstos"
T = "horas_trabalhadas"

# (empresa, colab, mes, ano, #fechamento, campos divergentes).
# Lista literal do m2_extrato.txt colado: [nao verificado por arquivo local].
PARES = (
    (2, 174, 9, 2026, 4825, (R,)),
    (2, 191, 9, 2026, 4518, (R,)),
    (2, 193, 9, 2026, 4801, (R,)),
    (2, 206, 9, 2026, 4418, (R,)),
    (2, 211, 9, 2026, 4648, (R,)),
    (2, 214, 9, 2026, 4668, (A,)),
    (2, 231, 9, 2026, 4699, (R,)),
    (2, 242, 9, 2026, 4503, (T, R)),
    (2, 252, 9, 2026, 4796, (R,)),
    (2, 269, 9, 2026, 4569, (R,)),
    (2, 272, 9, 2026, 4828, (A,)),
    (2, 278, 9, 2026, 4750, (T, R)),
    (2, 292, 9, 2026, 4671, (A, R)),
    (2, 331, 9, 2026, 4737, (R,)),
    (2, 349, 9, 2026, 4666, (R,)),
    (2, 355, 9, 2026, 4782, (A,)),
    (2, 371, 9, 2026, 4564, (R,)),
    (2, 444, 9, 2026, 4571, (R,)),
    (2, 489, 9, 2026, 4552, (R,)),
    (2, 529, 9, 2026, 4698, (R,)),
    (2, 532, 9, 2026, 4508, (R,)),
    (2, 592, 9, 2026, 4512, (R,)),
    (2, 616, 9, 2026, 4703, (R,)),
    (2, 634, 9, 2026, 4442, (R,)),
    (2, 735, 9, 2026, 4496, (R,)),
    (2, 788, 9, 2026, 4475, (R,)),
    (2, 789, 9, 2026, 4551, (R,)),
    (2, 833, 9, 2026, 4434, (R,)),
    (2, 849, 9, 2026, 4640, (P,)),
    (2, 859, 9, 2026, 4820, (A,)),
    (2, 865, 9, 2026, 4694, (R,)),
    (2, 872, 9, 2026, 4744, (R,)),
    (2, 885, 9, 2026, 4702, (R,)),
    (2, 915, 9, 2026, 4759, (R,)),
    (2, 923, 9, 2026, 4642, (R,)),
    (2, 941, 9, 2026, 4436, (R,)),
    (2, 945, 9, 2026, 4608, (R,)),
    (2, 301, 10, 2026, 5308, (T,)),
    (2, 899, 10, 2026, 5220, (T,)),
    (2, 191, 11, 2026, 6074, (T,)),
    (2, 369, 11, 2026, 5970, (T,)),
    (2, 415, 11, 2026, 6054, (T,)),
    (2, 476, 11, 2026, 5855, (T,)),
    (2, 788, 11, 2026, 6139, (T,)),
    (2, 899, 11, 2026, 6004, (T,)),
    (2, 961, 11, 2026, 6018, (T,)),
    (3, 82, 9, 2026, 4856, (R,)),
    (3, 87, 9, 2026, 4861, (R,)),
    (3, 114, 9, 2026, 4892, (R,)),
    (3, 133, 9, 2026, 4915, (A,)),
    (3, 145, 9, 2026, 4926, (R,)),
    (3, 165, 9, 2026, 4946, (R,)),
    (3, 502, 9, 2026, 4887, (R,)),
    (3, 638, 9, 2026, 4839, (P, R)),
    (3, 749, 9, 2026, 4910, (P, R)),
    (3, 868, 9, 2026, 4905, (R,)),
    (3, 749, 11, 2026, 5630, (T,)),
    (3, 911, 11, 2026, 6041, (T,)),
    (4, 50, 9, 2026, 4964, (A, R)),
)


class EscritaProibida(BaseException):
    pass


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass


def bloquear(*args, **kwargs):
    raise EscritaProibida()


def so_select(execute, sql, params, many, context):
    s = str(sql).strip()
    if (
        many
        or context["connection"].alias != "default"
        or not re.match(r"^SELECT\b", s, re.I)
        or ";" in s
        or "--" in s
        or "/*" in s
        or re.search(
            r"\b(INSERT|UPDATE|DELETE|MERGE|INTO|CREATE|ALTER|DROP|"
            r"TRUNCATE|COPY|CALL|DO|SET|RESET|GRANT|REVOKE|"
            r"NEXTVAL|SETVAL|PG_ADVISORY_LOCK|PG_TRY_ADVISORY_LOCK|"
            r"PG_ADVISORY_XACT_LOCK|PG_TRY_ADVISORY_XACT_LOCK|"
            r"LO_IMPORT|LO_EXPORT|PG_TERMINATE_BACKEND|PG_CANCEL_BACKEND|"
            r"PG_RELOAD_CONF|SET_CONFIG)\b|\bFOR\s+(SHARE|KEY)\b",
            s, re.I,
        )
    ):
        raise EscritaProibida()
    return execute(sql, params, many, context)


def sonda():
    saida = sys.stdout
    erros = 0
    pars = []
    totais_dia = Counter()
    membros = defaultdict(set)
    totais_par = Counter()
    dias_par = Counter()
    linhas_dia = 0
    etapa = "preparo"
    contexto = {}
    nivel = logging.root.manager.disable
    bytecode = _BYTECODE_ANTERIOR

    def emitir(tipo, **dados):
        def numero(x):
            if isinstance(x, Decimal):
                return format(x, "f")
            raise TypeError()

        saida.write(tipo + "|" + json.dumps(
            dados, ensure_ascii=False, sort_keys=True,
            default=numero, allow_nan=False,
        ) + "\n")

    def erro(onde, exc, **chaves):
        nonlocal erros
        erros += 1
        # Sem mensagem da excecao, modelo, SQL ou traceback.
        emitir(
            "ERRO " + str(erros),
            etapa=onde,
            escrita_bloqueada=isinstance(exc, EscritaProibida),
            **chaves,
        )

    def exigir(condicao):
        if not condicao:
            raise ValueError()

    def minutos(valor, campo):
        if valor is None:
            return None
        n = Decimal(str(valor))
        exigir(n.is_finite())
        return n if campo.startswith("minutos_") else n * 60

    try:
        sys.dont_write_bytecode = True
        logging.disable(logging.CRITICAL)

        with ExitStack() as stack, localcontext() as ctx:
            ctx.prec = 50
            stack.enter_context(redirect_stdout(Descartar()))
            stack.enter_context(redirect_stderr(Descartar()))

            from django.db import connection, connections
            from django.db.models import Model
            from django.db.models.query import QuerySet

            exigir(connection.settings_dict["NAME"] == "sombra")
            exigir(connection.schema_name == "juliani")

            for alias in connections:
                stack.enter_context(
                    connections[alias].execute_wrapper(so_select)
                )

            for classe, metodos in (
                (Model, ("save", "delete")),
                (QuerySet, (
                    "create", "update", "delete", "_raw_delete",
                    "bulk_create", "bulk_update", "get_or_create",
                    "update_or_create",
                )),
            ):
                for metodo in metodos:
                    stack.enter_context(
                        patch.object(classe, metodo, bloquear)
                    )

            with connection.cursor() as cursor:
                cursor.execute(
                    "SELECT current_database(), current_schema()"
                )
                exigir(cursor.fetchone() == ("sombra", "juliani"))

            from freezegun import freeze_time
            from django.contrib.auth import get_user_model
            from django.utils import timezone
            from chamados.juizes import tranca_em_lote
            from colaboradores.models import Colaborador
            from ponto.models import FechamentoMensal
            from ponto.janelas import janela_fechamento
            from ponto.services.dia_pago import (
                CAMPOS_DIA, linhas_do_periodo, soma_da_competencia,
            )
            from ponto.services.espelho import espelho_do_colab
            from ponto.management.commands.e6_oraculo import InsumosDoDono

            stack.enter_context(freeze_time(AGORA, tick=False))
            stack.enter_context(
                timezone.override("America/Sao_Paulo")
            )
            hoje = timezone.localtime(AGORA).date()

            sistema_pk = (
                get_user_model().objects.filter(username="sistema")
                .values_list("pk", flat=True).first()
            )
            exigir(sistema_pk is not None)
            stack.enter_context(
                tranca_em_lote(sistema_pk=sistema_pk)
            )

            exigir(len(PARES) == len({p[:4] for p in PARES}) == 59)
            exigir(
                Counter(p[0] for p in PARES) == {2: 46, 3: 12, 4: 1}
            )
            exigir(sum(len(p[5]) for p in PARES) == 65)
            exigir(all(
                set(p[5]) <= set(CAMPOS_DIA) for p in PARES
            ))

            emitir(
                "META",
                unidade="minutos",
                gravado_diario=False,
                selecao="todos os dias de janela_fechamento",
                fonte_lavra="app/ponto/services/dia_pago.py:833",
                fonte_gravado="app/ponto/models.py:398",
                fonte_dono=(
                    "app/ponto/management/commands/e6_oraculo.py:182"
                ),
                execucao_nesta_resposta="[nao verificado]",
            )

            for eid, cid, mes, ano, fid, campos in PARES:
                contexto = dict(
                    empresa=f"emp{eid}",
                    colab=f"col{cid}",
                    comp=f"{mes:02d}/{ano}",
                )
                donos = Counter()
                dias = 0
                inicio_erros = erros
                etapa = "preparo_par"

                try:
                    c = (
                        Colaborador.objects
                        .select_related("empresa").get(pk=cid)
                    )
                    exigir(c.empresa_id == eid)

                    fm = FechamentoMensal.objects.filter(
                        colaborador_id=cid, mes=mes, ano=ano,
                    ).first()
                    exigir(fm is not None and fm.pk == fid)

                    ini, fim = janela_fechamento(
                        mes, ano, c.empresa
                    )
                    exigir(fim >= ini)
                    dias = (fim - ini).days + 1

                    # Mesmas chamadas da m2 colada [nao verificado].
                    # app/ponto/services/dia_pago.py:833 e :550.
                    ds = linhas_do_periodo(c, ini, fim)
                    soma_lavra = soma_da_competencia(c, mes, ano)

                    gravado_comp = {
                        k: minutos(getattr(fm, k), k)
                        for k in campos
                    }

                    emitir(
                        "COMP",
                        **contexto,
                        fechamento=f"#{fm.pk}",
                        ini=ini.isoformat(),
                        fim=fim.isoformat(),
                        campos_anexo=list(campos),
                        gravado_diario=False,
                        gravado_competencia=gravado_comp,
                        soma_lavra_competencia=(
                            None if soma_lavra is None else {
                                k: minutos(soma_lavra[k], k)
                                for k in campos
                            }
                        ),
                        campos_divergentes_agora=(
                            None if soma_lavra is None else [
                                k for k in CAMPOS_DIA
                                if getattr(fm, k) is not None
                                and round(float(getattr(fm, k)), 2)
                                    != soma_lavra[k]
                            ]
                        ),
                    )

                    # Mesmo preparo do command:
                    # app/ponto/management/commands/e6_oraculo.py:316/:328.
                    esp = espelho_do_colab(
                        c, hoje, ini, fim, agora=AGORA
                    )
                    ins = InsumosDoDono(
                        c, ini, fim, hoje,
                        espelho=esp, agora=AGORA,
                    )

                    for offset in range(dias):
                        dia = ini + dt.timedelta(days=offset)
                        etapa = "dia"

                        try:
                            dp = ds.get(dia)
                            if dp is not None:
                                exigir(
                                    (dp.mes, dp.ano) == (mes, ano)
                                )

                            valores = {
                                k: (
                                    None if dp is None
                                    else minutos(getattr(dp, k), k)
                                )
                                for k in campos
                            }

                            # Carregador E juiz real, sem reproduzir regras:
                            # e6_oraculo.py:182/:190/:195.
                            dono = ins.dono(dia)
                            exigir(dono in (
                                "ESTRUTURA", "CADASTRO", "BATIDA",
                            ))

                            registros = [
                                dict(
                                    **contexto,
                                    dia=dia.isoformat(),
                                    campo=k,
                                    lavra=valores[k],
                                    gravado=None,
                                    dono=dono,
                                    unidade="minutos",
                                    gravado_estado=(
                                        "sem valor diario no fechamento"
                                    ),
                                    gravado_competencia=gravado_comp[k],
                                    dia_pago=(
                                        None if dp is None
                                        else f"#{dp.pk}"
                                    ),
                                    lavra_estado=(
                                        "sem lavratura do dia"
                                        if dp is None else
                                        "sem valor no campo"
                                        if valores[k] is None else
                                        "lavrado"
                                    ),
                                    divergencia_diaria_comprovada=False,
                                )
                                for k in campos
                            ]

                            for registro in registros:
                                emitir("DIA", **registro)
                                chave = (
                                    eid, registro["campo"], dono,
                                )
                                totais_dia[chave] += 1
                                membros[chave].add((cid, mes, ano))
                                linhas_dia += 1

                            # Uma contagem por DIA, mesmo com dois campos.
                            donos[dono] += 1

                        except Exception as exc:
                            erro(
                                "dia", exc, **contexto,
                                dia=dia.isoformat(),
                            )

                except Exception as exc:
                    erro(etapa, exc, **contexto)

                completo = (
                    erros == inicio_erros
                    and sum(donos.values()) == dias > 0
                )
                par = dict(
                    **contexto,
                    campos=list(campos),
                    donos=dict(sorted(donos.items())),
                    dias=dias,
                    dias_julgados=sum(donos.values()),
                    completo=completo,
                )
                pars.append(par)
                emitir("PAR", **par)

                if completo:
                    perfil = (
                        eid,
                        tuple(sorted(campos)),
                        tuple(sorted(donos)),
                    )
                    totais_par[perfil] += 1
                    dias_par[perfil] += dias

            etapa, contexto = "conferencia", {}
            exigir(len(pars) == 59)

            if erros == 0:
                exigir(all(p["completo"] for p in pars))
                exigir(sum(totais_par.values()) == 59)
                exigir(
                    sum(dias_par.values())
                    == sum(p["dias"] for p in pars)
                )
                exigir(
                    sum(totais_dia.values()) == linhas_dia
                )
                exigir(
                    linhas_dia == sum(
                        p["dias"] * len(p["campos"])
                        for p in pars
                    )
                )

    except BaseException as exc:
        erro(etapa, exc, **contexto)

    finally:
        logging.disable(nivel)
        sys.dont_write_bytecode = bytecode

    for (eid, campo, dono), n in sorted(totais_dia.items()):
        emitir(
            "TOT",
            unidade="dia_campo",
            empresa=f"emp{eid}",
            campo=campo,
            dono=dono,
            dias=n,
            pares_incidentes=len(membros[(eid, campo, dono)]),
        )

    for (eid, campos, perfil), n in sorted(totais_par.items()):
        emitir(
            "TOT",
            unidade="par",
            empresa=f"emp{eid}",
            campo=list(campos),
            dono=list(perfil),
            pares=n,
            dias=dias_par[(eid, campos, perfil)],
        )

    saida.write(
        f"FIM ok={int(erros == 0)} erros={erros}"
        f" pares={len(pars)}"
        f" pares_completos={sum(totais_par.values())}"
        f" esperado=59 linhas_dia_campo={linhas_dia}\n"
    )

    if erros:
        raise SystemExit(1)


sonda()
```

## 15/11 01:35 · sessao d3bab7ce6ebf · BATIDA-DEDUP-RETRATADA · verificador · origem=Code

**REPROVADO nesta volta.** Há incoerências nos retornos que a cura passa a alcançar e uma guarda existente que não preserva a remoção do filtro cru (`app/ponto/registro_batida.py:120`; `app/ponto/services/triagem_batida.py:716`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:31`).

As referências abaixo usam a numeração do HEAD. O teste novo, seus RED/GREEN e mutantes permanecem **[nao verificado]**. [nao verificado]

1. **Campos, significados, ordem e ID**

   | Campo/valor | Quem escreve/define; quem lê; conclusão |
   |---|---|
   | `retratada_em` | `retratar_batida` fornece o instante **da retratação**, e `anotar` persiste; desfazer restaura `None` pela mesma porta. A autoridade e o dedup temporal da porta leem apenas NULL/não NULL. Não comparam esse carimbo com a janela do toque (`app/ponto/registro_batida.py:274`; `app/ponto/registro_batida.py:252`; `app/ponto/services/retratar_humano.py:207`; `app/ponto/turnos.py:2876`; `app/ponto/registro_batida.py:109`). |
   | `timestamp` | As APIs obtêm `ts_efetivo` da autoridade de hora; a triagem captura `agora`; `registrar_batida` grava o argumento recebido. Dedup, chave exata e respostas leem esse instante (`app/api/views.py:507`; `app/api/views_core.py:493`; `app/ponto/services/triagem_batida.py:626`; `app/ponto/registro_batida.py:158`; `app/api/views.py:540`). |
   | `tipo` | Entra pelo request, mas as APIs podem substituí-lo **depois do dedup**, antes da porta. O campo persistido também pode ser invertido por `flip_tipo`, via `anotar`. Portanto, dedup da casca e porta podem perguntar por tipos diferentes, mesmo após c1 (`app/api/views.py:420`; `app/api/views.py:529`; `app/api/views.py:578`; `app/api/views.py:601`; `app/api/views.py:655`; `app/ponto/services/flip_batida.py:30`). |
   | Janela | APIs: ±5 minutos do fato; triagem: `[agora−5min, agora]`; porta, mesmo tipo: ±60 segundos. Todos usam limites **inclusivos**, `gte/lte`; continuam sendo perguntas temporalmente diferentes (`app/api/views.py:527`; `app/api/views_core.py:513`; `app/ponto/services/triagem_batida.py:663`; `app/ponto/registro_batida.py:110`; `app/ponto/turnos.py:2878`). |
   | Fuso/dia | A autoridade não converte os limites em dia nem consulta relógio. A entrada de hora torna timestamps aware; a configuração declara São Paulo e `USE_TZ=True`; apresentação usa `localtime`. Nesse caminho, não encontrei troca de instante por dia causada pela substituição (`app/ponto/turnos.py:2875`; `app/api/hora_do_aparelho.py:116`; `app/config/settings/base.py:93`; `app/config/settings/base.py:95`; `app/api/views.py:535`). |

   **A ordem efetiva da triagem não muda entre candidatas vivas.** Antes, `Meta.ordering=["colaborador","timestamp"]`; depois, a autoridade ordena por `timestamp`. Com colaborador fixo, ambos escolhem o menor instante; a unicidade colaborador/timestamp elimina empate. Antes sai o ID da primeira candidata, inclusive retratada; depois sai o ID da primeira **viva**, ou a execução continua se nenhuma existir (`app/ponto/models.py:105`; `app/ponto/models.py:106`; `app/ponto/services/triagem_batida.py:664`; `app/ponto/services/triagem_batida.py:671`; `app/ponto/turnos.py:2883`).

   Nas APIs continua saindo a candidata de **maior timestamp**, agora dentro do universo vivo proposto; `id`, horário e sequência vêm dessa linha (`app/api/views.py:532`; `app/api/views.py:538`; `app/api/views.py:541`; `app/api/views_core.py:518`; `app/api/views_core.py:524`).

   Quanto ao consumidor: o PWA online presente ignora o corpo e considera `r.ok` sucesso; o sincronizador apaga a pendência em todo `resp.ok`, sem usar o ID retornado. O upload servidor recebe `batida_id`, mas o cliente APK que ligaria esse campo ao retorno está **[nao verificado]** (`app/templates/ponto/bater_ponto_v2.html:327`; `app/static/pwa.js:158`; `app/static/pwa.js:162`; `app/api/views.py:836`; `app/api/views.py:842`).

2. **O que acontece depois que a batida chega à porta**

   | Caso | Resultado da porta |
   |---|---|
   | Retratada dentro de 60 s, **instante diferente** | A retratada não satisfaz o dedup. Sem outra viva, chave exata ou colisão impeditiva, cria uma nova viva. Não duplica a chave de banco (`app/ponto/registro_batida.py:109`; `app/ponto/registro_batida.py:120`; `app/ponto/registro_batida.py:157`; `app/ponto/models.py:106`). |
   | Chave exata colaborador/timestamp | Devolve a anterior, **inclusive retratada ou de outro tipo**. Não cria nem desfaz a retratação; portanto, pode continuar sem nova batida apurável (`app/ponto/registro_batida.py:116`; `app/ponto/registro_batida.py:120`; `app/ponto/registro_batida.py:122`; `app/ponto/turnos.py:2876`). |
   | Oposta viva na janela | Usa ±300 s com escala/marcos ou ±60 s sem eles. Salvo correspondência ao marco próprio, retorna `None, conflito` para o automático: **barra a materialização**. As três cascas passam `autoridade_humana=False` (`app/ponto/registro_batida.py:21`; `app/ponto/registro_batida.py:145`; `app/ponto/registro_batida.py:151`; `app/api/views.py:656`; `app/api/views_core.py:647`; `app/ponto/services/triagem_batida.py:704`). |

   **A resposta posterior não é coerente em todos os casos.** A porta não distingue criação de reaproveitamento no retorno. Após chave exata, as APIs anunciam `status=ok`, usando `tipo` e rótulo da variável local, mas timestamp/sequência do objeto devolvido. Se ele for de outro tipo, o payload pode contradizer o registro; se retratado, não informa que continua fora da apuração. A sequência retornada é a antiga, não a recém-calculada (`app/ponto/registro_batida.py:102`; `app/ponto/registro_batida.py:122`; `app/api/views.py:555`; `app/api/views.py:799`; `app/api/views.py:812`; `app/api/views.py:814`; `app/api/views_core.py:806`).

   **Há outro erro concreto na triagem:** após a recusa, ela procura oposta em ±60 s **sem excluir retratadas**. A porta pode ter recusado por uma viva a três minutos; a resposta então pode trazer `id=None`, ou apontar uma retratada próxima que não causou a recusa. As APIs usam a janela da porta e excluem retratadas nesse retorno (`app/ponto/services/triagem_batida.py:716`; `app/ponto/services/triagem_batida.py:723`; `app/ponto/registro_batida.py:31`; `app/ponto/registro_batida.py:141`; `app/api/views.py:672`; `app/api/views_core.py:663`).

   Esses ramos já existem no HEAD; retirar o retorno antecipado por retratada permite que mais chamadas os alcancem. A troca de consulta não garante, sozinha, resposta correta depois da porta (`app/ponto/services/triagem_batida.py:670`; `app/ponto/services/triagem_batida.py:702`; `app/ponto/services/triagem_batida.py:715`; `app/api/views.py:533`; `app/api/views.py:654`).

3. **Pendente de DP, origem e canal**

   `aprovada=False`, origem e canal, isoladamente, **não explicam divergência entre esses dedups**: as consultas antigas não os filtram; a autoridade também não; a porta temporal também não. Pendente de DP continua contando. `aprovada` é crivo do DP, enquanto `retratada_em` determina exclusão da apuração (`app/api/views.py:529`; `app/api/views_core.py:515`; `app/ponto/services/triagem_batida.py:664`; `app/ponto/turnos.py:2859`; `app/ponto/turnos.py:2876`; `app/ponto/registro_batida.py:107`; `app/ponto/models.py:63`).

   Permanecem as diferenças de janela, a possível correção de tipo entre casca e porta e a exceção da chave exata, que aceita até retratada. Não atribuo essas diferenças a um estado de aprovação/canal (`app/api/views.py:527`; `app/api/views.py:578`; `app/ponto/registro_batida.py:106`; `app/ponto/registro_batida.py:120`).

4. **Censo independente e classes estruturais**

   O censo encontrou **três dedups de chão** antes dos três chamadores de origem `app`: API original, API core e triagem. Coincide com os três sítios do T3; quarto sítio estático não encontrado. Caminhos dinâmicos não identificados permanecem **[nao verificado]** (`app/api/views.py:529`; `app/api/views.py:654`; `app/api/views_core.py:515`; `app/api/views_core.py:645`; `app/ponto/services/triagem_batida.py:664`; `app/ponto/services/triagem_batida.py:702`).

   Fora desse conjunto há reaproveitamento de materialização por ±20 minutos, guardas de vizinhas e chave exata de regularização externa. São controles existentes, mas não outro toque de chão (`app/chamados/juizes.py:235`; `app/chamados/juizes.py:243`; `app/chamados/services/materializacao.py:95`; `app/chamados/services/materializacao.py:101`; `app/chamados/services/regularizacao_ext.py:74`).

   A substituição usa uma autoridade existente; ela não acrescenta escritor, relógio ou trava. A criação encontrada permanece na porta, inclusive o bulk de ensaio (`app/ponto/turnos.py:2856`; `app/ponto/turnos.py:2875`; `app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:157`; `app/ponto/registro_batida.py:335`).

   **Derivação paralela está presente no retorno da triagem:** ela tenta reconstruir a causa da recusa com universo e janela diferentes. Também permanece a divergência entre instante do fato e recepção no envelope `ultima_batida.timestamp`, escrito com `agora`; isso pertence ao item T4 distinto, não foi corrigido por c1 (`app/ponto/services/triagem_batida.py:716`; `app/ponto/registro_batida.py:139`; `app/api/views.py:761`; `app/api/views_core.py:755`; `app/chamados/juizes.py:151`).

   Um selo restrito aos `SITIOS` enumerados deixaria um quarto sítio fora da lista sem cobertura, conforme a limitação relatada; implementação e mordida do teste novo **[nao verificado]**. Suite inteira, opus, smoke e eventual pouso antes desses pré-requisitos **[nao verificado]**. [nao verificado]

5. **Import local e uso de `Batida`**

   Não encontrei ciclo no trajeto estático: o topo de `turnos` importa bibliotecas e `core.dia_do_turno`; este último só constrói o tipo com `datetime/typing`. `batidas_apuraveis` apenas monta a consulta. Execução real do novo import **[nao verificado]** (`app/ponto/turnos.py:36`; `app/ponto/turnos.py:41`; `app/core/dia_do_turno.py:10`; `app/core/dia_do_turno.py:13`; `app/ponto/turnos.py:2875`).

   **`Batida` continua usado nos três módulos**, particularmente nas buscas de oposta após a porta; remover seu import seria errado (`app/api/views.py:674`; `app/api/views_core.py:665`; `app/ponto/services/triagem_batida.py:716`).

6. **Selos existentes: distinguir duas situações**

   **B5j não perde copiadores com esta cura.** Seu detector considera funções que alimentam o pareador, não qualquer dedup. A contagem em memória permanece **6 → 6**; os seis estão em `turnos` duas vezes, lastro, piso, meio período e fila por causa. O teto oito já deixa duas posições de folga no HEAD; essa folga não nasceu em c1 (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:123`; `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:191`; `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:217`; `app/ponto/turnos.py:1863`; `app/ponto/turnos.py:1901`; `app/ponto/services/lastro.py:261`; `app/ponto/services/piso_intervalo.py:39`; `app/escala/services/meio_periodo.py:158`; `app/chamados/services/fila_por_causa.py:190`).

   **O tripwire de bocais perde uma ocorrência e deveria guardar essa remoção.** No HEAD, sua regex encontra três filtros em `api/views`; com a troca, ficam dois. O teto continua sete, e a asserção usa `<=`: restaurar a consulta removida passa nesse selo. `views_core` e triagem sequer estão nessa allowlist (`app/api/views.py:296`; `app/api/views.py:529`; `app/api/views.py:674`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:13`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:20`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:25`; `app/ponto/tests/test_tripwire_bocais_apuracao.py:31`).

**Veredito final: REPROVADO**, pelos retornos inconsistentes detalhados no item 2 e pela guarda incompleta do item 6. A ordem da triagem e a contagem de pendentes de DP não são defeitos da substituição (`app/ponto/models.py:105`; `app/ponto/turnos.py:2859`; `app/ponto/turnos.py:2883`).

## 15/11 01:32 · sessao 9e8e05c97134 · - · - · origem=Ronald (tmux)

1. **Não há mudança na escolha entre batidas vivas da triagem.** Antes, `Meta.ordering=["colaborador","timestamp"]`; depois, `order_by("timestamp")`. Como a query restringe um colaborador e existe unicidade `(colaborador,timestamp)`, ambas devolvem o menor instante elegível; a diferença é remover retratadas (`app/ponto/models.py:105`, `app/ponto/models.py:106`, `app/ponto/services/triagem_batida.py:664`, `app/ponto/turnos.py:2883`). A afirmação de equivalência da entrega está correta para esse cenário; confirmação SQL executada [nao verificado].

2. **B5j não cai com esta cura.** Executei apenas suas funções AST em memória, simulando as três substituições em strings: copiadores `6 → 6`; os três módulos tocados contribuem `0 → 0`. O detector considera funções que chamam `parear_turnos`/`_turnos_do_colab_puro`, não qualquer função do módulo (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:123`). Os seis copiadores atuais são:
   - `app/ponto/turnos.py:1863`;
   - `app/ponto/turnos.py:1901`;
   - `app/ponto/services/lastro.py:261`;
   - `app/ponto/services/piso_intervalo.py:39`;
   - `app/escala/services/meio_periodo.py:158`;
   - `app/chamados/services/fila_por_causa.py:190`.

3. **B5j já tem folga anterior: teto 8 para seis atuais.** O comentário promete catraca fundada no número contado, mas a implementação exige somente `total <= 8`, permitindo dois copiadores adicionais (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:184`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:191`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:215`). Não atribuir essa folga à cura: ela já existe no HEAD; a lei apenas declara “8 copiadores ... não cresce” (`CLAUDE.md:163`).

4. **O selo de bocais deveria apertar sua contagem para guardar a remoção.** A regex encontra três filtros em `api/views.py`, nas linhas `296`, `529`, `674`; a substituição proposta leva a dois. Entretanto, o teto permanece sete e a asserção é somente `<=`, aumentando a folga de quatro para cinco (`app/ponto/tests/test_tripwire_bocais_apuracao.py:20`, `app/ponto/tests/test_tripwire_bocais_apuracao.py:25`, `app/ponto/tests/test_tripwire_bocais_apuracao.py:31`). Restaurar o filtro removido continuará passando nesse selo (`app/api/views.py:529`, `app/ponto/tests/test_tripwire_bocais_apuracao.py:31`). `views_core` e `triagem_batida` não constam da allowlist (`app/ponto/tests/test_tripwire_bocais_apuracao.py:13`).

5. **Sem troca de instante por dia no predicado novo.** A autoridade aplica `timestamp__gte` e `timestamp__lte`, inclusive nas duas pontas; filtra somente retratação, contando pendentes de DP e sem restrição por origem/canal (`app/ponto/turnos.py:2859`, `app/ponto/turnos.py:2876`). Ela não lê relógio, não escreve e não trava (`app/ponto/turnos.py:2875`). As APIs fornecem `ts_efetivo` da autoridade de hora; a triagem usa seu `agora` capturado (`app/api/views.py:497`, `app/api/views.py:507`, `app/ponto/services/triagem_batida.py:626`).

6. **Retratação é carimbo do ato, não o instante da batida.** `retratar_batida` anota `aprovada=False` e `retratada_em=timezone.now()`; o gravador material é `anotar`/`save(update_fields=...)` (`app/ponto/registro_batida.py:274`, `app/ponto/registro_batida.py:252`). O modelo declara `aprovada` como crivo do DP e retratação como exclusão da geometria/apuração (`app/ponto/models.py:63`, `app/ponto/models.py:64`).

7. **Fuso consistente para o caminho inspecionado.** A configuração declara `America/Sao_Paulo` e `USE_TZ=True`; a regra documental exige UTC no banco e apresentação UTC−3 (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`, `CLAUDE.md:146`). A autoridade da hora torna timestamps ingênuos aware; aceita o aparelho com atraso inclusivo de zero a 48 horas e retorna `agora` fora disso (`app/api/hora_do_aparelho.py:116`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:153`). Persistência/fuso efetivos de produção [nao verificado].

## 15/11 01:32 · sessao cfe31aeba724 · - · - · origem=Ronald (tmux)

1. **Retratada dentro de 60s:** a porta exclui a anterior no filtro (`app/ponto/registro_batida.py:107-113`). Sem outra viva do mesmo tipo, chave exata ou oposta impeditiva, cria linha viva (`app/ponto/registro_batida.py:120`, `app/ponto/registro_batida.py:145`, `app/ponto/registro_batida.py:157`). Não grava duplicata da chave; o banco exige unicidade por colaborador+timestamp (`app/ponto/models.py:106`).

2. **Chave exata:** devolve a linha anterior, inclusive retratada ou de outro tipo, sem revivê-la e sem criar nova (`app/ponto/registro_batida.py:116-122`). A cura portanto não garante nova batida apurável nesse caso (`app/ponto/turnos.py:2876`).

3. **Oposta na janela:** a janela é ±300s com escala/marcos e ±60s sem eles (`app/ponto/registro_batida.py:21-32`). Considera só opostas vivas e recusa o automático com `(None, conflito)`, salvo correspondência ao marco próprio (`app/ponto/registro_batida.py:139-151`). A tolerância do marco próprio é inclusiva, `<=5min` (`app/ponto/registro_batida.py:41`, `app/ponto/registro_batida.py:48`). As três cascas passam `autoridade_humana=False` (`app/api/views.py:656`, `app/api/views_core.py:647`, `app/ponto/services/triagem_batida.py:704`). Esse caminho continua barrando materialização (`app/ponto/registro_batida.py:151`).

4. **Resposta rica após retorno existente:** a porta não distingue “criada” de “reaproveitada” no contrato de retorno (`app/ponto/registro_batida.py:102`, `app/ponto/registro_batida.py:115`, `app/ponto/registro_batida.py:122`, `app/ponto/registro_batida.py:161`). As APIs devolvem `status=ok`, tipo e rótulo calculados da variável `tipo`, mas timestamp e sequência da linha retornada (`app/api/views.py:799-815`, `app/api/views_core.py:793-809`). Se a chave exata devolver retratada/outro tipo, o payload não informa a retratação e pode anunciar um tipo diferente do objeto (`app/ponto/registro_batida.py:116-122`, `app/api/views.py:812-815`). A sequência devolvida é a gravada na linha antiga, não `sequencia_jornada` recém-calculada (`app/api/views.py:555`, `app/api/views.py:659`, `app/api/views.py:814`). A linha reutilizada também recebe os efeitos posteriores, como foto e geofence (`app/api/views.py:769-781`).

5. **Triagem possui divergência adicional na resposta de oposta:** após `None`, procura oposta crua em ±60s, sem excluir retratada (`app/ponto/services/triagem_batida.py:715-725`). A porta pode ter recusado por uma viva a 3min, pois usa até ±300s (`app/ponto/registro_batida.py:21-32`, `app/ponto/registro_batida.py:139-151`). Logo a resposta pode trazer `id=None`, ou apontar uma retratada que não causou a recusa (`app/ponto/services/triagem_batida.py:716-725`). As APIs consultam a janela da porta e excluem retratada (`app/api/views.py:672-687`, `app/api/views_core.py:663-678`).

6. **Pendente DP/origem/canal:** as três queries antigas não filtram esses estados (`app/api/views.py:529-532`, `app/api/views_core.py:515-518`, `app/ponto/services/triagem_batida.py:664-669`). `batidas_apuraveis` também não os filtra; exclui somente retratada (`app/ponto/turnos.py:2876-2883`). A porta usa o mesmo critério no dedup e na colisão, e nem esse filtro na chave exata (`app/ponto/registro_batida.py:107-120`, `app/ponto/registro_batida.py:139-144`). Portanto `aprovada=False`, origem ou canal, isoladamente, não são divergências entre essas consultas (`app/ponto/turnos.py:2876-2883`, `app/ponto/registro_batida.py:107-120`).

7. **Divergências remanescentes:** API usa ±5min, triagem `[agora−5min,agora]`, porta ±60s para mesmo tipo (`app/api/views.py:527-532`, `app/ponto/services/triagem_batida.py:663-669`, `app/ponto/registro_batida.py:106-113`). Na API, dedup usa o tipo solicitado antes da correção pela autoridade; a porta recebe o tipo corrigido (`app/api/views.py:529-532`, `app/api/views.py:570-601`, `app/api/views.py:654-655`). Essas diferenças já existem no HEAD; a cura informada não as altera [nao verificado].

_5477 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
