# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 23:12** · publicado em **05/11/2026 23:17** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 23:12 · sessao 7fcfc9f0d68a · C20-FIO-MUDO · verificador · origem=Code

**Não certificaria a obra como completa.** P3 continua parcial; a reabertura da disputa acrescenta risco de ordem de travas e efeitos sobre outros leitores; os cinco atos de perda ainda não têm cobertura demonstrada. (`app/chamados/models.py:461`, `app/chamados/models.py:1483`, `app/chamados/models.py:1492`, `app/chamados/models.py:513`, `app/chamados/services/fio.py:891`, `app/chamados/services/fio.py:845`)

Execução dos testes, eficácia dos mutantes, reprodução das corridas e resultado em produção: **[não verificado]**. [nao verificado]

**1. P1–P6 e os descartes da triagem**

| Achado | Avaliação do delta |
|---|---|
| **P1 — eleição paralela da canônica** | **Resolve a eleição no grupo misto.** Agora reúne vivas locais e órfãs por motivo e consulta `canonica_do_grupo`; desapareceu a precedência automática da local. O ramo “todas daqui” ainda pula qualquer normalização, relevante na ressuscitação posterior. (`app/chamados/models.py:491`, `app/chamados/models.py:496`, `app/chamados/models.py:498`; `app/chamados/services/fio.py:873`) |
| **P2 — retratação antes da costura aceita** | **Resolve dentro da adoção.** Canônica externa só permite retratação após retorno positivo da porta; recusa dá `continue`. Não resolve o mesmo padrão no migrador, que continua retratando antes de reapontar. (`app/chamados/models.py:501`, `app/chamados/models.py:505`, `app/chamados/models.py:514`; `app/chamados/services/fio.py:943`, `app/chamados/services/fio.py:947`) |
| **P3 — transação da reabertura direta** | **Resolve em parte.** Serializa a adoção do mesmo chamado e relê seu estado, mas a leitura inicial, a transição e a ressuscitação de `reabrir()` continuam anteriores a essa unidade. (`app/chamados/models.py:462`, `app/chamados/models.py:1424`, `app/chamados/models.py:1483`, `app/chamados/models.py:1492`, `app/chamados/models.py:1493`) |
| **P4 — duplicata respondida ainda viva** | **Resolve a seleção indevida e corrige a documentação; não resolve suas consequências nos demais leitores.** A adoção exclui `VIA_DUPLICATA`, enquanto C6 continua podendo considerá-la viva pela resposta. (`app/chamados/models.py:446`, `app/chamados/models.py:477`; `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`) |
| **P5 — continente fechado** | **Resolve em parte.** Reabre a disputa da canônica efetivamente movida. Não executa esse efeito quando a canônica já está aqui; tampouco demonstra acesso real pelo colaborador ou coerência dos outros chamados/perguntas do continente. (`app/chamados/models.py:499`, `app/chamados/models.py:512`, `app/chamados/models.py:514`; `app/chamados/tests/test_c20_fio_mudo.py:229`) |
| **P6 — R19a vacuamente verde sem cobrança** | **Resolve essa vacuidade.** Agora exige cobrança existente antes de conferir contagem e destino da validada. (`app/chamados/tests/test_c20_fio_mudo.py:272`, `app/chamados/tests/test_c20_fio_mudo.py:273`, `app/chamados/tests/test_c20_fio_mudo.py:275`) |

Os motivos dos descartes:

- **R19c:** sustenta apenas adiamento, não cobertura. O teste continua executando hoje e consultando amanhã; não tenta adotar órfã futura. C5 determina a data do fato, sem impor por si só “não futuro”. (`app/chamados/tests/test_c20_fio_mudo.py:301`; `app/chamados/juizes.py:80`, `app/chamados/juizes.py:91`)
- **Tranca:** a delegação ao C6 procede; a alegação de mordida por R19a não basta. R19a prepara validação, que retorna antes do degrau de tranca. Matar um mutante que remove C6 inteiro não demonstra cobertura da retirada isolada da tranca. (`app/chamados/tests/test_c20_fio_mudo.py:268`; `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1140`)
- **Queries:** sustenta “sem medição”, não “sem risco”. A busca continua carregando apenas `disputa`, sem o mapa de identidade empregado pelo migrador. O selo de corrida usa outro módulo; a captura do chokepoint começa depois do `abrir` de preparação. Estouro efetivo: **[não verificado]**. (`app/chamados/models.py:478`; `app/chamados/services/fio.py:929`; `app/chamados/tests/test_c2_corrida_do_abrir.py:56`; `app/ponto/tests/test_selo_chokepoint_escrita.py:312`, `app/ponto/tests/test_selo_chokepoint_escrita.py:325`)
- **D±1:** “ampliar seria juiz novo” não se sustenta como necessidade técnica: E4-c já decide compatibilidade entre datas adjacentes na mesma jornada. A seleção atual não chega a essas candidatas. Obrigatoriedade dessa ampliação pelo aval literal: **[não verificado]**. (`app/chamados/models.py:475`; `app/chamados/juizes.py:1957`, `app/chamados/juizes.py:1991`)
- **L-113:** copiar a cerca existente não concilia a lei posterior. A adoção sai quando a competência foi entregue; L-113 permite nascimento em exportada e registra a queda da Pauta DP como pré-condição. Não considero a divergência resolvida pelo descarte. (`app/chamados/models.py:472`; `app/docs/LEIS.md:150`)
- **Nunca-bateu:** a dispensa de questionário tem fundamento; a exclusão integral sem conferir pai/agrupamento não está provada por R19d. A própria adesão prevê agrupar os furos no pai, e o teste só exige cobrança, veredito e ausência de perguntas. (`app/chamados/services/adesao.py:19`, `app/chamados/services/adesao.py:102`, `app/chamados/services/adesao.py:112`; `app/chamados/tests/test_c20_fio_mudo.py:118`)
- **Gancho no perdedor:** permanece incompleto. As portas gerais de retratação e reapontamento não reparam o chamado que perde sua pergunta. (`app/chamados/services/fio.py:891`, `app/chamados/services/fio.py:845`)
- **Universo:** os números reproduzidos somam 495; retirados três, restam 492, enquanto o aval escrito continua 494. Reconciliação individual: **[não verificado]**. (`app/chamados/tests/test_c20_fio_mudo.py:1`, `app/chamados/tests/test_c20_fio_mudo.py:13`)

O achado de relógio também permanece: a adoção consulta C6, cujo ramo de janela pode ler `date.today()` fora de parâmetro recebido da borda. É leitura preexistente ampliada pela composição, não corrigida pelo delta. (`app/chamados/models.py:489`; `app/chamados/juizes.py:1197`, `app/chamados/juizes.py:562`)

**2. Quem escreve e quem lê**

| Campo/ato | Escritores e leitores relevantes |
|---|---|
| **`Disputa.fechada_em`** | `fechar` preenche; `reabrir_admin` e `reabrir_sistema` limpam. Leem `aberta`, lookup de disputa, fio/API do colaborador e reconciliador. (`app/chamados/models.py:1952`, `app/chamados/models.py:2154`, `app/chamados/models.py:2178`, `app/chamados/models.py:1856`; `app/chamados/services/fio.py:131`; `app/api/views.py:1008`; `app/chamados/reconciliador.py:1149`) |
| **`fechada_por`** | Os mesmos três caminhos escrevem/limpam; os receivers de fechamento usam o autor para retomada/materialização. (`app/chamados/models.py:1953`, `app/chamados/models.py:2155`, `app/chamados/models.py:2179`; `app/chamados/signals.py:113`, `app/chamados/signals.py:140`) |
| **`resolucao`** | Fechamento escreve; as duas reaberturas limpam; `limpar_resolucao_residual` é outra porta preexistente específica. Leem a nota de fechamento, a cauda da materialização e a guarda de idempotência do fechamento. (`app/chamados/models.py:1954`, `app/chamados/models.py:2156`, `app/chamados/models.py:2180`, `app/chamados/models.py:2264`, `app/chamados/models.py:1912`; `app/chamados/signals.py:45`; `app/chamados/services/materializacao.py:1250`) |
| **`motivo_fechamento`** | Fechamento preenche; reaberturas limpam. Leem a idempotência do fechamento e o contador de disputa vazia. (`app/chamados/models.py:1955`, `app/chamados/models.py:2157`, `app/chamados/models.py:2181`, `app/chamados/models.py:1913`; `app/chamados/services/vigia_fio.py:734`) |
| **`Pergunta.via_resolucao`** | O delta usa `retratar_duplicata → carimbar_identidade → _escrever_carimbo`. Existem ainda os escritores preexistentes de relavra e encerramento por fato. Leem a nova seleção, C6, estado e ressuscitação. (`app/chamados/services/fio.py:891`; `app/chamados/models.py:2651`, `app/chamados/models.py:2735`, `app/chamados/models.py:2764`, `app/chamados/models.py:477`, `app/chamados/models.py:2593`, `app/chamados/models.py:1546`; `app/chamados/services/relavra_via.py:123`; `app/chamados/juizes.py:1171`) |
| **`Pergunta.chamado_destino`** | Nascimento/identidade em `Pergunta.abrir` e costura em `reapontar_resolvedora`; o delta chama a costura. Leem âncora, montagem do fio, lookup de disputa, gate e migrador. (`app/chamados/models.py:2541`, `app/chamados/models.py:2567`, `app/chamados/models.py:501`; `app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:117`, `app/chamados/services/fio.py:135`, `app/chamados/services/fio.py:236`, `app/chamados/services/fio.py:931`; `app/chamados/models.py:344`) |
| **`Chamado.status_local` sob trava** | A adoção somente lê; a escrita continua em `_gravar_estado` e no nascimento. A leitura travada não atualiza `self`; a porta de costura relê seu destino. (`app/chamados/models.py:462`, `app/chamados/models.py:222`, `app/chamados/models.py:1021`; `app/chamados/services/fio.py:763`) |
| **`pergunta_reatada`** | Gate e adoção chamam `core.observ.evento`, escritor da auditoria. No gate, `objeto_id` é o chamado que fecha; na adoção, é o que abre. Há leitura pela auditoria genérica; consumidor operacional específico: **[não verificado]**. (`app/chamados/models.py:423`, `app/chamados/models.py:506`; `app/core/observ.py:94`; `app/core/views.py:705`) |
| **Ocorrência de `reabrir_sistema`** | Passa por `nota.anexar_ocorrencia` e vai ao **chamado original da disputa**, não ao destino adotante. Sem `objeto_ref`, não recebe deduplicação própria. O fio carrega esses followups. (`app/chamados/models.py:2183`, `app/chamados/models.py:2185`; `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:103`; `app/chamados/services/fio.py:172`) |

**Não identifiquei segundo escritor direto novo fora dessas portas.** O delta acrescenta chamadas; os donos do estado fechado já incluem `reabrir_sistema`. Isso não transforma `via_resolucao` em campo com um único sítio de atribuição em toda a cópia. (`app/chamados/models.py:501`, `app/chamados/models.py:513`, `app/chamados/models.py:516`; `app/chamados/tests/test_selo_resolucao_um_escritor.py:32`; `app/chamados/services/relavra_via.py:123`)

Os leitores, porém, passam a enxergar combinações distintas:

- **Disputa aberta com pai terminal é possível.** `reabrir_sistema` não reabre o pai; o receiver do pai só atua no fechamento. Isso não invalida automaticamente a pergunta, cuja âncora pode ser o novo destino vivo, mas precisa ser distinguido da acessibilidade do continente. (`app/chamados/models.py:2178`, `app/chamados/models.py:2185`; `app/chamados/signals.py:28`; `app/chamados/services/fio.py:117`)
- Reabrir a disputa torna **outras perguntas dela** elegíveis aos recortes de fio/reconciliação; não somente a adotada. Também pode fazer a disputa voltar a bloquear seu chamado original vivo. (`app/chamados/services/fio.py:214`; `app/chamados/reconciliador.py:1038`, `app/chamados/reconciliador.py:1482`; `app/chamados/juizes.py:2080`, `app/chamados/juizes.py:2084`)
- `fio_vivo` e vigia continuam reduzindo C6, independentemente de a disputa estar aberta. Já a adoção exclui duplicatas antes de C6; o migrador não faz essa exclusão. Uma duplicata respondida pode entrar na eleição do migrador e ser ignorada pela adoção. (`app/chamados/juizes.py:1242`; `app/chamados/services/vigia_fio.py:165`; `app/chamados/models.py:477`; `app/chamados/services/fio.py:931`, `app/chamados/services/fio.py:935`)
- Para o fechamento citado como `fechar_se_todas_respondidas`, o caminho equivalente conferido é `_fechar_disputa_se_quitada`: consulta C7, que exige ausência de respostas sem veredito e de fio vivo. Não fecha apenas porque todas responderam. Correspondência com outro símbolo exato: **[não verificado]**. (`app/chamados/services/materializacao.py:1224`, `app/chamados/services/materializacao.py:1238`; `app/chamados/models.py:1885`)

**3. Ordem de trava e transação**

**`abrir()`** chama a adoção dentro de seu `atomic`, já segurando o colaborador; nascimento e adoção ficam na transação externa. O `atomic(savepoint=False)` interno não libera essas travas nem cria confirmação independente. (`app/chamados/models.py:952`, `app/chamados/models.py:955`, `app/chamados/models.py:1024`, `app/chamados/models.py:461`)

**`reabrir()` direto** não envolve todo o ato: consulta estado sem trava, grava a transição em seu núcleo transacional, ressuscita e depois adota. A trava nova impede duas adoções simultâneas do mesmo destino, mas não impede duas execuções de aprovarem anteriormente a reabertura. (`app/chamados/models.py:1424`, `app/chamados/models.py:274`, `app/chamados/models.py:1483`, `app/chamados/models.py:1492`, `app/chamados/models.py:1493`)

**A afirmação “não trava Disputa, ordem intacta” é insuficiente.** A adoção segura Chamado, salva Pergunta e chama um `save` da Disputa; esse `UPDATE` participa das travas de escrita. No caminho de `abrir`, isso ocorre também sob a trava do Colaborador. (`app/chamados/models.py:462`, `app/chamados/models.py:955`, `app/chamados/models.py:513`, `app/chamados/models.py:2182`; `app/chamados/services/fio.py:847`)

Há um **ciclo estático possível**:

1. T1 segura o chamado A e carrega uma candidata cuja disputa D aparece fechada. Outro ato reabre D e confirma. (`app/chamados/models.py:462`, `app/chamados/models.py:478`, `app/chamados/models.py:2154`, `app/chamados/models.py:2166`)
2. T2 declara hora em A; sua transação obtém D agora aberta e mantém sua trava. Reutilizando uma pergunta sem destino, tenta costurá-la em A e espera T1. (`app/chamados/services/declaracao_texto.py:302`, `app/chamados/services/declaracao_texto.py:310`, `app/chamados/services/declaracao_texto.py:314`, `app/chamados/services/declaracao_texto.py:320`; `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:502`; `app/chamados/services/fio.py:762`)
3. T1 usa seu objeto antigo de D fechada e tenta `reabrir_sistema`, esperando D: **Chamado→Disputa contra Disputa→Chamado**. Reprodução do deadlock: **[não verificado]**. (`app/chamados/models.py:512`, `app/chamados/models.py:513`, `app/chamados/models.py:2182`)

Não usei o comentário do selo como prova automática de Disputa→Colaborador: no emissor, encontrar a disputa retorna antes do ramo que chama `abrir`. O selo é intraprocedural, considera apenas dois modelos e não cobre essa composição nem travas por escrita. (`app/chamados/services/disputa_emissao.py:502`, `app/chamados/services/disputa_emissao.py:504`; `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:14`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:23`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:85`)

Também existe trava explícita conjunta de Pergunta e Disputa na relavra, inclusive fechada no comportamento padrão; deve entrar no censo de ordem. (`app/chamados/services/relavra_via.py:185`, `app/chamados/services/relavra_via.py:221`)

**Corrida com fechamento permanece:** a adoção decide sobre D carregada anteriormente; `reabrir_sistema` não relê seu estado. `fechar` também decide antes de seu bloco de escrita e grava os mesmos quatro campos. Portanto, não há decisão mutuamente protegida entre reabrir e fechar; qual escrita prevaleceria numa execução concreta: **[não verificado]**. (`app/chamados/models.py:478`, `app/chamados/models.py:512`, `app/chamados/models.py:2172`, `app/chamados/models.py:2182`, `app/chamados/models.py:1911`, `app/chamados/models.py:1937`, `app/chamados/models.py:1956`)

**4. Contraexemplos e idempotência**

- **Canônica daqui + órfã respondida:** se ambas têm resposta e a local é mais antiga, a local vence. Não há costura, evento ou reabertura; a órfã vira duplicata, pode continuar viva por C6 e fica na âncora terminal, excluída das adoções seguintes. (`app/chamados/services/fio.py:873`; `app/chamados/models.py:477`, `app/chamados/models.py:499`, `app/chamados/models.py:514`; `app/chamados/juizes.py:1150`)
- **Canônica órfã em D1, locais em D2:** só D1 reabre. D2 recebe retratações sem atualização do continente; se a canônica já é local, nem a disputa dela passa pela reabertura nova. (`app/chamados/models.py:499`, `app/chamados/models.py:512`, `app/chamados/models.py:514`)
- **Retratar local já respondida:** o carimbo não garante sua morte. A porta documenta que a resposta pode mantê-la viva; o gate continua consultando C6 sobre ancoradas sem excluir duplicata. Logo, carimbar as irmãs não demonstra “uma única viva”. (`app/chamados/models.py:2716`, `app/chamados/models.py:327`, `app/chamados/models.py:351`; `app/chamados/juizes.py:1150`)
- **Disputa de outro chamado vivo:** a proteção examina a âncora efetiva da pergunta. Destino explícito terminal permite candidatura mesmo com `disputa.chamado` vivo diferente; reabrir esse continente pode alterar o bloqueio daquele outro chamado. (`app/chamados/services/fio.py:117`; `app/chamados/models.py:487`, `app/chamados/models.py:513`; `app/chamados/juizes.py:2080`)
- **Segunda adoção simples:** para no grupo “todas daqui”, sem repetir costura/evento. Um segundo `reabrir` após o primeiro terminar normalmente também é recusado pela transição já viva. Isso não cobre concorrência nem ciclo posterior de fechamento/reabertura. (`app/chamados/models.py:496`, `app/chamados/models.py:1408`, `app/chamados/models.py:1478`)
- **Ressuscitação posterior:** uma duplicata local sem resposta pode entrar no universo das materializadas sem texto, ser descarimbada e voltar viva quando o fato acusa novamente. Se a canônica também estiver local, a adoção pula o grupo inteiro e não refaz o dedup. Contraexemplo condicionado a reabertura admitida; reprodução: **[não verificado]**. (`app/chamados/models.py:1525`, `app/chamados/models.py:1546`, `app/chamados/models.py:1549`, `app/chamados/models.py:1595`, `app/chamados/models.py:2754`, `app/chamados/models.py:496`)

A trilha também tem limites: `evento` é best effort; a ocorrência de reabertura não tem chave própria. Ausência de repetição de evento/nota não é demonstrada apenas pelo retorno `[]`. (`app/core/observ.py:98`, `app/core/observ.py:139`; `app/chamados/models.py:2185`; `app/chamados/services/nota.py:103`)

**5. Mordida dos testes novos** [nao verificado]

| Teste | Avaliação estática | [nao verificado]
|---|---|
| **R08d** | Tem mordida para P1: prova órfã viva respondida, cobrança e local existentes, destino da canônica e carimbo da local. Não cobre duas respondidas, canônica local ou estado das duas disputas. (`app/chamados/tests/test_c20_fio_mudo.py:180`, `app/chamados/tests/test_c20_fio_mudo.py:186`, `app/chamados/tests/test_c20_fio_mudo.py:191`) |
| **R08e** | Tem mordida para seleção da duplicata: exige que ela continue viva, permaneça no terminal e que a canônica seja adotada. Também evidencia que a duplicata viva permanece fora da cura. (`app/chamados/tests/test_c20_fio_mudo.py:208`, `app/chamados/tests/test_c20_fio_mudo.py:214`, `app/chamados/tests/test_c20_fio_mudo.py:215`) |
| **R08f** | Morde a ausência de reabertura de `fechada_em`, mas não a promessa completa de acesso. A fixture cria disputa sem chamado; não exercita ocorrência, pai terminal/vivo ou limpeza dos outros campos. O montador HTML exclui disputas sem chamado. (`app/chamados/tests/test_c20_fio_mudo.py:54`, `app/chamados/tests/test_c20_fio_mudo.py:220`, `app/chamados/tests/test_c20_fio_mudo.py:229`; `app/chamados/services/fio.py:181`) |
| **R08g** | **O mock intercepta a função realmente chamada:** o import é local ao método e busca o símbolo no módulo patchado. Porém, não comprova chamada com candidata/destino esperados; uma saída antecipada pode passar sem testar recusa. Também não confere preservação dos destinos. (`app/chamados/tests/test_c20_fio_mudo.py:240`, `app/chamados/tests/test_c20_fio_mudo.py:245`; `app/chamados/models.py:474`, `app/chamados/models.py:501`) |
| **R08i** | Prova primeira adoção e segunda chamada retornando `[]`, com contagem estável. Não confere eventos, notas, carimbos, continentes ou ciclo de reabertura. (`app/chamados/tests/test_c20_

[truncado em 20000 de 22873 caracteres]

## 05/11 23:11 · sessao 1af9bb312b57 · F-346 · verificador · origem=Code

**R07/R08 fecham as lacunas anunciadas dos oito campos, por análise estática. O selo AST melhorou, mas continua permitindo falsos negativos e falsos positivos.** (`app/ferias/test_remarcar_limpa_janela_f346.py:112`, `app/ferias/test_remarcar_limpa_janela_f346.py:129`, `app/ferias/test_remarcar_limpa_janela_f346.py:153`, `app/ferias/test_remarcar_limpa_janela_f346.py:175`)

Executei o helper, o método sintético e o detector de relógio por extração AST em memória, sem importar os módulos Django. Execução Django, mutantes em banco e comportamento em produção: **[nao verificado]**; `django` e `freezegun` não estão disponíveis neste ambiente. Conferência independente de hash/pai: **[nao verificado]**. [nao verificado]

1. **Sim: os oito campos agora têm valores discriminantes.**

R07 carrega o objeto **antes** do UPDATE concorrente simulado. Os três valores novos diferem dos carregados; depois da remarcação, recarrega e compara os três. Portanto, incluir isoladamente `confirmacao`, `confirmado_em` ou `obs` em `update_fields`, ou voltar ao save completo, sobrescreveria uma sentinela e falharia na comparação. Isso é dedução estática, sem execução dos mutantes. (`app/ferias/models.py:313`, `app/ferias/models.py:314`, `app/ferias/models.py:318`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/test_remarcar_limpa_janela_f346.py:117`)

R08 exige estas mudanças **no objeto recarregado do banco**; retirar isoladamente qualquer um dos cinco campos da persistência deixa um valor antigo discriminante. (`app/ferias/test_remarcar_limpa_janela_f346.py:31`, `app/ferias/test_remarcar_limpa_janela_f346.py:124`, `app/ferias/test_remarcar_limpa_janela_f346.py:128`)

| Campo | Antes → exigido por R08 |
|---|---|
| `data_inicio` | `ini` → `ini + 30 dias` |
| `data_fim` | `ini + 9 dias` → `ini + 36 dias` |
| `dias` | `10` → `7` |
| `aviso_emitido` | `False` → `True` |
| `status` | `concluido` → `agendado` |

Valores e comparação: `app/ferias/test_remarcar_limpa_janela_f346.py:31`, `app/ferias/test_remarcar_limpa_janela_f346.py:124`, `app/ferias/test_remarcar_limpa_janela_f346.py:129`.

**Ainda faltam sentinelas explícitas de preservação para `periodo_aquisitivo_id`, `agendado_por_id` e `criado_em`.** Esses campos existem, ficam fora do save da troca e não são comparados após a remarcação em R01–R08. A PK participa das seleções/recargas, mas também não recebe uma comparação independente de preservação. (`app/ferias/models.py:299`, `app/ferias/models.py:310`, `app/ferias/models.py:319`, `app/ferias/services.py:392`, `app/ferias/test_remarcar_limpa_janela_f346.py:43`, `app/ferias/test_remarcar_limpa_janela_f346.py:112`, `app/ferias/test_remarcar_limpa_janela_f346.py:129`)

R08 testa diretamente o serviço: seu estado inicial `concluido` seria recusado pela porta HTTP de remarcação. Isso não prejudica a discriminação da persistência, mas limita o que o caso demonstra sobre a porta. (`app/ferias/test_remarcar_limpa_janela_f346.py:124`, `app/ferias/test_remarcar_limpa_janela_f346.py:127`, `app/ferias/views.py:649`)

2. **Resultado em memória sobre `services.py`: `['_trocar_janela']`.**

Esse resultado corresponde à atribuição existente na troca; o helper examina somente o arquivo indicado pelo teste e suas funções síncronas. Não constitui prova de ausência de escritores nas formas que ele ignora. (`app/ferias/test_remarcar_limpa_janela_f346.py:133`, `app/ferias/test_remarcar_limpa_janela_f346.py:153`, `app/ferias/test_remarcar_limpa_janela_f346.py:189`, `app/ferias/services.py:379`)

Confirmei estas classes de **falsos negativos** em memória:

| Forma sintética | Resultado e motivo |
|---|---|
| `[ag.data_inicio, ag.data_fim] = [i, f]`; tupla aninhada | `[]`: só desempacota uma camada de `ast.Tuple`; não percorre listas nem alvos aninhados. (`app/ferias/test_remarcar_limpa_janela_f346.py:159`) |
| `async def z(...): ag.data_inicio = i` | `[]`: exige `ast.FunctionDef`. Escritas no nível do módulo também ficam fora desse universo. (`app/ferias/test_remarcar_limpa_janela_f346.py:153`) |
| `self.data_inicio = i`, sendo `self` um AG | `[]`: exclusão textual de `self`. (`app/ferias/test_remarcar_limpa_janela_f346.py:161`) |
| `setattr(self.ag, "data_inicio", i)`; `self.agendamentos.update(data_inicio=i)`; `periodo.agendamentos.update(data_inicio=i)` | `[]`: a raiz `self`/`periodo` é excluída mesmo quando o destinatário final é AG. (`app/ferias/test_remarcar_limpa_janela_f346.py:137`, `app/ferias/test_remarcar_limpa_janela_f346.py:143`, `app/ferias/test_remarcar_limpa_janela_f346.py:170`, `app/ferias/test_remarcar_limpa_janela_f346.py:173`) |
| `qs.bulk_update(xs, campos)` ou `fields=campos` | `[]`: não resolve a variável dos campos. (`app/ferias/test_remarcar_limpa_janela_f346.py:174`) |
| Alias de `setattr`, `builtins.setattr(...)`, `ag.__setattr__(...)`, `ag.__dict__["data_inicio"] = i`, `getattr(qs, "update")(...)` | `[]`: não correspondem às formas literais reconhecidas. (`app/ferias/test_remarcar_limpa_janela_f346.py:160`, `app/ferias/test_remarcar_limpa_janela_f346.py:167`, `app/ferias/test_remarcar_limpa_janela_f346.py:170`, `app/ferias/test_remarcar_limpa_janela_f346.py:173`) |

Confirmei também estes **falsos positivos** em memória:

- `pa.data_inicio = i`, a variante anotada com valor e `setattr(pa, "data_inicio", i)` contam mesmo se `pa` for `PeriodoAquisitivo`; outro modelo ou um dicionário com `.update(dias=i)` também conta. Não há resolução de tipo/alias, e as exclusões diferem entre atribuição e chamadas. (`app/ferias/test_remarcar_limpa_janela_f346.py:137`, `app/ferias/test_remarcar_limpa_janela_f346.py:160`, `app/ferias/test_remarcar_limpa_janela_f346.py:163`, `app/ferias/test_remarcar_limpa_janela_f346.py:173`)
- `qs.update(obs="dias")` e `qs.filter(obs="dias").update(status="concluido")` contam: a constante `"dias"` pode aparecer em **qualquer parte da chamada**, inclusive filtro ou valor de outro campo. (`app/ferias/test_remarcar_limpa_janela_f346.py:175`)
- `qs.update(**{"obs": "x"})` e `campo="obs"; setattr(ag, campo, "x")` contam. Esse excesso é deliberado e documentado como conservador. (`app/ferias/test_remarcar_limpa_janela_f346.py:168`, `app/ferias/test_remarcar_limpa_janela_f346.py:170`, `app/ferias/test_remarcar_limpa_janela_f346.py:174`)
- Uma função interna que atribui a data faz contar **a interna e a externa**, pois `ast.walk(fn)` atravessa definições aninhadas. (`app/ferias/test_remarcar_limpa_janela_f346.py:152`, `app/ferias/test_remarcar_limpa_janela_f346.py:155`)

3. **Não cobrem cada ramo: falta uma sentinela de `AugAssign`.**

Todas as asserções existentes do método sintético passaram na execução extraída em memória. A cobertura declarada está assim: (`app/ferias/test_remarcar_limpa_janela_f346.py:201`)

| Forma | Caso existente |
|---|---|
| `Assign` simples | Sim, `a/b`. (`app/ferias/test_remarcar_limpa_janela_f346.py:202`) |
| `AugAssign` | **Não**; o helper aceita, mas o método não contém exemplo. (`app/ferias/test_remarcar_limpa_janela_f346.py:156`, `app/ferias/test_remarcar_limpa_janela_f346.py:201`) |
| `AnnAssign` sem/com valor | Sim, `n/o`. (`app/ferias/test_remarcar_limpa_janela_f346.py:216`, `app/ferias/test_remarcar_limpa_janela_f346.py:217`) |
| `setattr` constante/variável | Sim, `d/m`. (`app/ferias/test_remarcar_limpa_janela_f346.py:206`, `app/ferias/test_remarcar_limpa_janela_f346.py:215`) |
| `update` kwargs/`**` | Sim, `e/k`; `g` protege o filtro `periodo_aquisitivo`. (`app/ferias/test_remarcar_limpa_janela_f346.py:207`, `app/ferias/test_remarcar_limpa_janela_f346.py:211`, `app/ferias/test_remarcar_limpa_janela_f346.py:214`) |
| `bulk_update` | Sim, lista literal em `f`. (`app/ferias/test_remarcar_limpa_janela_f346.py:208`) |
| Alheio | Sim, atribuição em `periodo` e UPDATE em `PeriodoAquisitivo`. (`app/ferias/test_remarcar_limpa_janela_f346.py:205`, `app/ferias/test_remarcar_limpa_janela_f346.py:213`) |

Isso não cobre todas as condições internas: faltam, por exemplo, `_raiz` com `Subscript`/retorno `None`, exclusões de chamadas por `self`/`periodo` e `setattr` constante em campo alheio. (`app/ferias/test_remarcar_limpa_janela_f346.py:143`, `app/ferias/test_remarcar_limpa_janela_f346.py:170`, `app/ferias/test_remarcar_limpa_janela_f346.py:201`)

4. **Não identifiquei leitura nova de relógio fora do freeze; UTC é compatível com a configuração copiada.**

O detector executado em memória retornou **`0`**. O arquivo não contém chamadas diretas a `today/now/localdate/localtime`; R07 constrói um instante literal, e R07/R08 pertencem à classe congelada. As leituras do serviço e de `status_por_data` ficam dentro da chamada feita pelos testes congelados. (`app/core/tests/test_selo_teste_sem_relogio.py:32`, `app/ferias/test_remarcar_limpa_janela_f346.py:23`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/services.py:380`, `app/ferias/models.py:280`)

O selo, sozinho, é menos forte: encontrar o texto `freeze_time` já faz retornar zero para o arquivo inteiro; ele não verifica o alcance do congelamento. Aqui há também o decorador efetivo na classe. (`app/core/tests/test_selo_teste_sem_relogio.py:28`, `app/core/tests/test_selo_teste_sem_relogio.py:34`, `app/ferias/test_remarcar_limpa_janela_f346.py:23`)

`confirmado_em` é `DateTimeField`, o literal é aware em UTC, e a configuração declara `USE_TZ=True` e `America/Sao_Paulo`. A comparação é entre instantes, sem exigir identidade do objeto `tzinfo`; UTC versus outro offset equivalente não fragiliza essa asserção. O teste pressupõe timestamps aware: portabilidade para `USE_TZ=False`/outro backend permanece **[nao verificado]**. (`app/ferias/models.py:318`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/test_remarcar_limpa_janela_f346.py:117`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`)

5. **Escritores e leitores dos cinco campos semeados por UPDATE direto:**

| Campo | Escritores no produto | Leitores específicos localizados |
|---|---|---|
| `aviso_emitido` | Default; execução; aprovação; troca. (`app/ferias/models.py:305`, `app/ferias/services.py:323`, `app/ferias/services.py:439`, `app/ferias/services.py:380`) | KPI/filtro, aviso da situação, mensagem da remarcação e template. (`app/ferias/views.py:26`, `app/ferias/views.py:77`, `app/ferias/situacao.py:257`, `app/ferias/views.py:671`, `app/templates/ferias/_secao_colab.html:121`) |
| `status` | Default; histórico; cancelamentos DP/admin; troca; alinhamento do cron. (`app/ferias/models.py:309`, `app/ferias/services.py:282`, `app/ferias/services.py:336`, `app/ferias/services.py:361`, `app/ferias/services.py:384`, `app/ferias/services.py:518`) | Guardas do modelo/HTTP, consultas, materialização e template. (`app/ferias/models.py:324`, `app/ferias/views.py:649`, `app/ferias/services.py:51`, `app/ponto/services/ausencia.py:1164`, `app/templates/ferias/_secao_colab.html:120`) |
| `confirmacao` | Default; pedido; resposta confirmar/recusar. (`app/ferias/models.py:314`, `app/ferias/services.py:486`, `app/ferias/services.py:496`, `app/ferias/services.py:499`) | Guarda de envio, seleção da resposta, atendimento e template. (`app/ferias/views.py:838`, `app/ferias/views.py:880`, `app/chamados/views.py:628`, `app/templates/ferias/_secao_colab.html:122`) |
| `confirmado_em` | Pedido limpa; confirmação carimba. (`app/ferias/services.py:487`, `app/ferias/services.py:500`) | R07 e teste de confirmação. Leitor específico de runtime: **[nao verificado]**. (`app/ferias/test_remarcar_limpa_janela_f346.py:117`, `app/ferias/test_confirmacao_agendamento.py:76`) |
| `obs` | Histórico, execução, aprovação e cancelamento admin. (`app/ferias/services.py:284`, `app/ferias/services.py:323`, `app/ferias/services.py:441`, `app/ferias/services.py:360`) | Cancelamento admin lê para concatenar o carimbo; R07 confere preservação. (`app/ferias/services.py:360`, `app/ferias/test_remarcar_limpa_janela_f346.py:117`) |

**O `.update()` direto não executa o override `AgendamentoFerias.save()` nem emite os sinais de save; portanto não dispara a guarda/sincronização por esse caminho e não invalida a premissa das sentinelas.** O override concentra esses efeitos, e a própria cópia documenta o UPDATE sem signal. (`app/ferias/models.py:434`, `app/chamados/signals.py:80`, `app/colaboradores/services/colaborador.py:81`)

Já o `save(update_fields=...)` posterior, dentro da troca, executa guarda e sincronização normalmente. Triggers ou customizações presentes somente no banco implantado: **[nao verificado]**. (`app/ferias/services.py:392`, `app/ferias/models.py:435`, `app/ferias/models.py:437`)

## 05/11 23:09 · sessao 37fe8504fe3c · - · - · origem=Ronald (tmux)

**P3 está parcialmente resolvido:** a adoção da mesma linha passa a serializar, mas a reabertura inteira continua fora dessa unidade. `reabrir()` lê o estado sem trava, grava o novo estado em outro `atomic`, ressuscita perguntas e somente depois entra no `atomic(savepoint=False)` da adoção. Portanto, duas reaberturas ainda podem aprovar as guardas e registrar a transição antes da serialização nova. (`app/chamados/models.py:1424`, `app/chamados/models.py:1483`, `app/chamados/models.py:274`, `app/chamados/models.py:1492`, `app/chamados/models.py:1493`, `app/chamados/models.py:461`)

**Estado transacional dos chamadores:**

- `abrir()` chama a adoção dentro de seu `atomic`, já segurando o colaborador; o nascimento e a adoção pertencem à transação externa. O `savepoint=False` não solta essa trava nem cria uma fronteira independente. (`app/chamados/models.py:952`, `app/chamados/models.py:955`, `app/chamados/models.py:1021`, `app/chamados/models.py:1024`, `app/chamados/models.py:461`)
- `reabrir()` direto não estabelece transação sobre o ato inteiro. Sem transação externa, `_gravar_estado`, ressuscitação e adoção são etapas distintas; com chamador externo transacional, herdam a unidade dele. (`app/chamados/models.py:1413`, `app/chamados/models.py:1483`, `app/chamados/models.py:1492`, `app/chamados/models.py:1493`)
- A nova leitura travada confirma apenas o estado do banco; não atualiza a instância `self`. A costura consulta novamente o destino por sua própria porta. (`app/chamados/models.py:462`, `app/chamados/models.py:464`, `app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:763`)

**“Não trava Disputa” não demonstra preservação da ordem:** `reabrir_sistema()` executa um `UPDATE` da disputa enquanto a adoção segura o chamado e, no caminho de `abrir`, também o colaborador. A sequência nova inclui, portanto, chamado→pergunta→disputa, embora não exista `select_for_update` explícito de disputa no helper. (`app/chamados/models.py:462`, `app/chamados/models.py:501`, `app/chamados/services/fio.py:847`, `app/chamados/models.py:513`, `app/chamados/models.py:2182`, `app/chamados/models.py:955`)

**Ciclo estático factível, sem depender dos ramos excludentes do emissor:**

1. T1 adota para `ChA`, segura sua linha e carrega uma candidata `P1` cuja disputa `D` aparece fechada no snapshot. (`app/chamados/models.py:462`, `app/chamados/models.py:475`, `app/chamados/models.py:478`)
2. Outro ato reabre `D` e confirma essa alteração; seu chamado pai pode ser `ChB`, distinto de `ChA`. (`app/chamados/models.py:2154`, `app/chamados/models.py:2166`)
3. T2 declara hora em `ChA`. Seu `atomic` externo chama `disputa_para_declaracao`, que trava agora `D` aberta e retorna pelo ramo existente. A trava permanece até terminar o `atomic` da declaração. (`app/chamados/services/declaracao_texto.py:302`, `app/chamados/services/declaracao_texto.py:310`, `app/chamados/services/disputa_emissao.py:563`, `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:502`)
4. Uma candidata reutilizável sem destino — pode ser `P2`, de outro marco, deixando `P1` livre — leva T2 à costura de `ChA`, que espera sua linha travada por T1. (`app/chamados/services/declaracao_texto.py:226`, `app/chamados/services/declaracao_texto.py:313`, `app/chamados/services/declaracao_texto.py:320`, `app/chamados/services/fio.py:762`)
5. T1 usa seu snapshot fechado e tenta atualizar `D`, esperando a trava de T2: chamado→disputa contra disputa→chamado. Reprodução do deadlock: **[não verificado]**. (`app/chamados/models.py:512`, `app/chamados/models.py:513`, `app/chamados/models.py:2182`)

Não se deve usar isoladamente o comentário “Disputa→Colaborador” como prova desse ciclo: no `_abrir_ou_agregar_disputa`, encontrar a disputa retorna antes do `ChamadoColaborador.abrir`; no caminho da âncora, encontrar a disputa segue para pergunta e análise do chamado. (`app/chamados/services/disputa_emissao.py:502`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:1195`, `app/chamados/services/disputa_emissao.py:1203`)

**A trava de disputa também existe por JOIN:** `desfazer_carimbo_sem_lastro` trava pergunta e disputa com `select_for_update(of=('self', 'disputa'))`, inclusive disputa fechada no parâmetro padrão. Esse sítio precisa entrar no censo real de ordem. (`app/chamados/services/relavra_via.py:185`, `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:245`)

**Corrida com fechamento permanece:** a adoção decide reabrir pelo objeto de disputa carregado anteriormente; `reabrir_sistema` também decide pelo objeto recebido, sem reler ou comparar o estado gravado. `fechar()` igualmente decide pelas informações da instância antes de seu `atomic`, e grava os mesmos quatro campos. As portas não fornecem exclusão mútua entre essas decisões; prevalência concreta de um fechamento concorrente: **[não verificado]**. (`app/chamados/models.py:478`, `app/chamados/models.py:512`, `app/chamados/models.py:2172`, `app/chamados/models.py:2182`, `app/chamados/models.py:1911`, `app/chamados/models.py:1937`, `app/chamados/models.py:1952`, `app/chamados/models.py:1956`)

A ocorrência de reabertura é inserida no chamado **da disputa**, não necessariamente no novo destino; não recebe `objeto_ref`, portanto não obtém a chave de deduplicação da porta de notas. Duas instâncias antigas fechadas podem repetir essa ocorrência. (`app/chamados/models.py:2183`, `app/chamados/models.py:2185`, `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:102`, `app/chamados/services/nota.py:103`)

Os receivers de pergunta agendam o julgamento de becos em `on_commit`; portanto não fecham o beco no meio da adoção transacional. Os receivers de disputa fechada, por sua vez, podem reconciliar o pai e materializar perguntas durante o fechamento. (`app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/chamados/signals.py:41`, `app/chamados/signals.py:42`, `app/chamados/signals.py:124`, `app/chamados/signals.py:140`)

**O selo não prova essa composição:** ele examina `select_for_update` somente para raízes `Colaborador`/`DisputaSupervisao` e somente dentro da mesma função; não contempla os `UPDATE`s descritos, o JOIN com raiz `PerguntaDisputa` nem o encadeamento entre métodos. (`app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:14`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:23`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:85`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:105`)

## 05/11 23:08 · sessao 1219f33ddbbf · - · - · origem=Ronald (tmux)

**Testes — análise estática; execução, RED, mutantes e corridas: [não verificado].** [nao verificado]

| Caso | O que comprova e o que falta |
|---|---|
| **R08d** | Exercita órfã respondida versus local sem resposta; exige cobrança, pergunta local e vivacidade da órfã; verifica reapontamento da órfã e carimbo da local. Tem mordida para P1, mas não cobre local canônica respondida, duas respondidas, disputa local fechada ou acesso posterior. (`app/chamados/tests/test_c20_fio_mudo.py:173`, `:180`, `:186`, `:191`) |
| **R08e** | A preparação carimba uma duplicata respondida e exige que continue viva por C6; ao final exige duplicata imóvel e canônica reapontada. Morde a seleção indevida da duplicata respondida. Não prova que todos os leitores deixam de contá-la como fio vivo: C6 continua priorizando resposta. (`app/chamados/tests/test_c20_fio_mudo.py:204`, `:208`, `:214`; `app/chamados/juizes.py:1150`, `:1171`) |
| **R08f** | Morde a ausência do `reabrir_sistema`: exige reapontamento e `fechada_em=None`. A disputa de teste nasce **sem chamado**, logo não exercita ocorrência, chamado terminal/vivo da disputa, limpeza dos outros três campos ou acesso real do colaborador. (`app/chamados/tests/test_c20_fio_mudo.py:54`, `:220`, `:227`, `:229`) |
| **R08g** | **O mock intercepta a função certa**: patch em `chamados.services.fio.reapontar_resolvedora`; o helper faz import local desse módulo e chama o nome importado dentro do contexto do patch. Entretanto, não usa `assert_called`, não exige vivacidade das duas candidatas nem preservação dos destinos; uma saída antecipada do helper permitiria aprovação vacuosa. No fluxo pretendido, morde retratação anterior à recusa. (`app/chamados/tests/test_c20_fio_mudo.py:238`, `:240`, `:245`; `app/chamados/models.py:474`, `:489`, `:501`) |
| **R08i** | A primeira adoção é comprovada pelo destino; a segunda chamada exige `[]` e contagem estável. Cobre a segunda adoção sequencial simples. Não conta eventos, ocorrências, carimbos ou reaberturas, nem cobre concorrência ou ciclo fechar/reabrir. (`app/chamados/tests/test_c20_fio_mudo.py:248`, `:256`, `:259`, `:261`) |
| **R19a** | O assert novo elimina a vacuidade apontada em P6: agora exige cobrança e verifica que a validada não mudou de destino nem ganhou irmã. Continua sendo controle de **validação**, não de tranca. (`app/chamados/tests/test_c20_fio_mudo.py:268`, `:272`, `:275`) |

**Descartes da triagem**

- **R19c fraco: descarte apenas como adiamento.** O teste continua rodando o vigia de hoje e apenas consultando ausência de pergunta amanhã; não semeia órfã futura nem tenta abertura futura. C5 determina o dia, mas seu primeiro degrau não impõe “dia ≤ hoje”. (`app/chamados/tests/test_c20_fio_mudo.py:301`; `app/chamados/juizes.py:80`, `:91`)
- **Tranca: justificativa insuficiente.** R19a prepara `validada_em`; C6 retorna morto por validação antes de alcançar tranca. Um mutante que removesse somente o degrau de tranca não seria morto por esse cenário. Remover C6 inteiro e falhar em R19a não prova cobertura individual de tranca. (`app/chamados/tests/test_c20_fio_mudo.py:268`; `app/chamados/juizes.py:1136`, `:1140`)
- **Queries: risco permanece sem medição.** O selo de corrida usa módulo `sem_previsao_escala`, que sai na primeira guarda C20; o chokepoint chama `abrir` na preparação, antes da captura de queries. Estouro efetivo: **[não verificado]**. (`app/chamados/tests/test_c2_corrida_do_abrir.py:56`; `app/chamados/models.py:459`; `app/ponto/tests/test_selo_chokepoint_escrita.py:312`, `:325`)
- **D±1: argumento “ampliar seria juiz novo” não demonstrado.** C5 apenas entrega uma data; E4-c existente já decide a compatibilidade de jornadas entre D e D±1. Buscar candidatas adjacentes e consultar E4-c não exige necessariamente outro juiz. A busca atual mantém igualdade estrita; obrigação de ampliar perante o aval literal: **[não verificado]**. (`app/chamados/juizes.py:80`, `:1957`, `:1991`; `app/chamados/models.py:475`)
- **L-113: descarte não resolve a divergência.** A guarda segue exigindo competência não entregue; L-113 revoga pauta DP como pré-condição e permite nascimento em competência exportada. Copiar a cerca antiga demonstra precedente técnico, não conciliação com a lei posterior. (`app/chamados/models.py:472`; `app/docs/LEIS.md:150`)
- **Nunca-bateu: fila procede, exclusão integral não provada.** A lei/código dispensa questionário e prevê agrupamento dos furos no pai de adesão; R19d só exige cobrança, veredito e ausência de perguntas, sem pai/agrupamento. (`app/chamados/services/adesao.py:19`, `:102`, `:112`; `app/ponto/services/cartorio.py:1157`, `:1268`; `app/chamados/tests/test_c20_fio_mudo.py:118`)
- **Universo: permanece inconsistente.** Os números escritos somam 495; retirar os três nunca-bateu dá 492, enquanto o contrato continua 494. Reconciliação individual: **[não verificado]**. (`app/chamados/tests/test_c20_fio_mudo.py:1`, `:13`)

**Idempotência e reabertura**

- A segunda adoção sequencial simples para no grupo composto só por perguntas locais; não repete costura ou evento nesse ramo. (`app/chamados/models.py:496`)
- Um segundo `reabrir` após o primeiro ter deixado o chamado vivo é recusado pela transição, antes de ressuscitar/adotar. Isso difere de duas execuções concorrentes ou de fechar e reabrir novamente. (`app/chamados/models.py:1408`, `:1478`, `:1492`)
- **Contraexemplo adicional de ciclo:** uma duplicata local sem resposta pode ser ressuscitada num fechamento/reabertura posterior: o universo inclui materializadas sem texto, descarimba a cópia para consultar C6 e depois limpa a via no banco. Se a canônica também estiver local, a adoção pula o grupo “só daqui”, preservando as duas novamente vivas quando o dia acusa. É contraexemplo estático condicionado a uma reabertura admitida; reprodução: **[não verificado]**. (`app/chamados/models.py:1525`, `:1546`, `:1549`, `:1595`, `:2754`, `:496`; `app/chamados/juizes.py:1181`)

**Cinco atos de perda**

| Ato | Estado da cobertura |
|---|---|
| **Reabrir** | Melhorou a adoção de órfã e a disputa fechada da canônica movida; o atomic novo envolve só adoção, posterior a `_gravar_estado` e ressuscitação. R08r cobre execução simples. (`app/chamados/models.py:461`, `:1483`, `:1492`; `app/chamados/tests/test_c20_fio_mudo.py:150`) |
| **Retratar duplicata** | P2 foi curado dentro da adoção: move antes de retratar. A porta geral continua apenas carimbando identidade, sem reparar o chamado perdedor; o migrador existente ainda retrata antes de tentar costura. (`app/chamados/models.py:501`, `:516`; `app/chamados/services/fio.py:891`, `:943`, `:947`) |
| **Validação** | Nenhum gancho C20 novo; o caminho existente mantém atomic do carimbo/desfecho e recalcula por evento. Validação mata legitimamente no primeiro degrau de C6. (`app/chamados/services/validacao.py:395`, `:405`; `app/chamados/juizes.py:1136`) |
| **Morte por leitura** | Nenhum gancho C20 novo; C6 continua decidindo tranca, identidade, marco/célula; reconciliação consulta C6 e separa tranca antes de carimbar. (`app/chamados/juizes.py:1140`, `:1171`, `:1177`; `app/chamados/reconciliador.py:651`, `:657`) |
| **Reapontar** | A adoção usa a porta correta e a porta protege o destino; continua ausente guarda/reparação geral para o chamado que perde a pergunta. (`app/chamados/models.py:501`; `app/chamados/services/fio.py:837`, `:845`) |

A classe de testes usa `TestCase`; os novos cenários não executam explicitamente callbacks de commit, embora o save da pergunta agende fechamento de becos por `on_commit`. Efeitos posteriores desse fechamento: **[não verificado]** pelos testes novos. (`app/chamados/tests/test_c20_fio_mudo.py:22`, `:32`; `app/chamados/signals.py:191`, `:202`)

## 05/11 23:08 · sessao 790368a804d1 · - · - · origem=Ronald (tmux)

Achados estáticos, sem execução de testes ou acesso a produção **[não verificado]**. [nao verificado]

- **Não surgiu escritor direto novo dos campos auditados.** A adoção chama `reapontar_resolvedora`, `retratar_duplicata → carimbar_identidade`, `reabrir_sistema` e `evento`; não atribui diretamente esses estados. (`app/chamados/models.py:501`, `app/chamados/models.py:506`, `app/chamados/models.py:513`, `app/chamados/models.py:516`; `app/chamados/services/fio.py:891`)

- **Estado fechado da disputa:** `fechar` grava `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; `reabrir_admin` e `reabrir_sistema` limpam os quatro juntos; `limpar_resolucao_residual` também escreve `resolucao`, pela porta específica do passivo. (`app/chamados/models.py:1952`, `app/chamados/models.py:2154`, `app/chamados/models.py:2178`, `app/chamados/models.py:2264`) A cascata de desligamento viva já usa `_d.fechar`; os comentários de testes que a descrevem como escritor por `.update()` estão defasados. (`app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/desligamento.py:142`; `app/chamados/tests/test_porta_disputa_ciclo.py:51`)

- **Leitores desse estado:** `aberta` deriva de `fechada_em`; `_disputa_do_chamado` exige disputa aberta; fio do colaborador carrega perguntas das abertas, e API usa o mesmo recorte; reconciliador seleciona/reconfere `fechada_em` antes de fechar. (`app/chamados/models.py:1855`; `app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:135`, `app/chamados/services/fio.py:211`; `app/api/views.py:1008`; `app/chamados/reconciliador.py:1149`, `app/chamados/reconciliador.py:1482`) `fechada_por` alimenta retomada/materialização nos signals; `resolucao` alimenta a nota do fechamento e `_fechar_disputa_se_quitada`; `motivo_fechamento` alimenta o contador `disputa_vazia`. (`app/chamados/signals.py:45`, `app/chamados/signals.py:113`, `app/chamados/signals.py:140`; `app/chamados/services/materializacao.py:1250`; `app/chamados/services/vigia_fio.py:734`)

- **A ocorrência de reabertura vai ao chamado original da disputa**, não ao novo destino da pergunta. `reabrir_sistema` não reabre esse chamado, não muda `disputa.chamado` e não restringe a reabertura às perguntas adotadas. Pode, portanto, deixar disputa aberta cujo pai continua terminal e tornar elegíveis outras perguntas daquele continente. (`app/chamados/models.py:2178`, `app/chamados/models.py:2183`, `app/chamados/models.py:2185`; `app/chamados/services/fio.py:181`, `app/chamados/services/fio.py:214`) O signal de pai tem apenas ramo de fechamento e retorna quando `fechada_em` está vazio. (`app/chamados/signals.py:28`)

- **Nota e evento têm garantias diferentes.** A nota usa a porta `anexar_ocorrencia`; sem `objeto_ref`, é `create` sem deduplicação própria. Sua existência depende da guarda de reabertura. (`app/chamados/models.py:2172`, `app/chamados/models.py:2185`; `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:103`) `pergunta_reatada` é escrito pela porta genérica `evento`, que é best effort e pode retornar `None`. Os dois chamadores usam significados distintos para `objeto_id`: chamado que fecha no gate, chamado que abre na adoção. (`app/chamados/models.py:423`, `app/chamados/models.py:506`; `app/core/observ.py:98`, `app/core/observ.py:139`) Fio lê `followups`; consumidor de produção específico de `pergunta_reatada`: **[não verificado]**. (`app/chamados/services/fio.py:172`, `app/chamados/services/fio.py:176`)

- **`chamado_destino` tem dois caminhos autorizados já existentes:** nascimento/identidade em `PerguntaDisputa.abrir` e costura em `fio.reapontar_resolvedora`. A porta de identidade pode reapontar a última pergunta do fato sem passar pela função mockada em R08g. (`app/chamados/models.py:2522`, `app/chamados/models.py:2541`, `app/chamados/models.py:2567`; `app/chamados/services/fio.py:845`; `app/core/tests/test_contract_escritores_estado.py:60`) Leitores incluem âncora, montagem do fio, lookup de disputa e migrador. (`app/chamados/services/fio.py:117`, `app/chamados/services/fio.py:135`, `app/chamados/services/fio.py:236`, `app/chamados/services/fio.py:931`)

- **`via_resolucao` não possui só `_escrever_carimbo` como sítio em toda a cópia:** há escrita na porta de encerramento por fato e em `relavra_via` preexistentes. O delta passa pela porta de identidade. (`app/chamados/models.py:2651`, `app/chamados/models.py:2735`, `app/chamados/models.py:2764`; `app/chamados/services/relavra_via.py:123`; `app/chamados/services/fio.py:891`)

Contraexemplos relevantes:

1. **Canônica DAQUI + órfã respondida:** é possível se a daqui também tem resposta e menor PK. `canonica_do_grupo` escolhe a daqui; a adoção pula costura, evento e reabertura e retrata a órfã. A órfã respondida pode continuar viva pelo C6 e fica presa na âncora terminal, agora excluída das próximas adoções. (`app/chamados/services/fio.py:873`; `app/chamados/models.py:477`, `app/chamados/models.py:499`, `app/chamados/models.py:514`; `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`)

2. **Retratar pergunta daqui já respondida não garante uma só viva.** A irmã local respondida pode continuar viva mesmo após `VIA_DUPLICATA`; a própria porta documenta isso. O delta não relê C6 depois da retratação. (`app/chamados/models.py:514`, `app/chamados/models.py:2716`; `app/chamados/juizes.py:1150`) Isso pode conservar duas vivas no destino e fazer a duplicata respondida segurar o gate, pois `_ancoradas` filtra validação, não via. (`app/chamados/models.py:327`, `app/chamados/models.py:351`, `app/chamados/models.py:402`)

3. **Disputas diferentes:** quando a canônica órfã pertence a D1 e a local a D2, apenas D1 reabre; a irmã de D2 pode virar duplicata sem atualização do continente D2. Quando a canônica já é local, nem a disputa dela é reaberta. (`app/chamados/models.py:498`, `app/chamados/models.py:512`, `app/chamados/models.py:514`)

4. **Disputa de outro chamado vivo:** a guarda protege a âncora efetiva da pergunta, não `disputa.chamado`. Com destino explícito terminal e container vivo diferente, a pergunta é candidata e o delta pode reabrir a disputa desse outro container. (`app/chamados/services/fio.py:117`; `app/chamados/models.py:481`, `app/chamados/models.py:487`, `app/chamados/models.py:513`) `disputa_viva_do_chamado` passa a enxergar a disputa aberta e suas perguntas independentemente do destino delas, podendo alterar o bloqueio de fechamento do container vivo. (`app/chamados/juizes.py:2080`, `app/chamados/juizes.py:2084`)

5. **Migrador continua com universo diferente:** ele não exclui `VIA_DUPLICATA`; chama C6 sobre todas as resolvedoras. Uma duplicata respondida excluída pela adoção pode entrar na eleição do migrador. Além disso, o migrador continua retratando antes da costura, diferentemente da correção local P2. (`app/chamados/services/fio.py:931`, `app/chamados/services/fio.py:935`, `app/chamados/services/fio.py:941`, `app/chamados/services/fio.py:943`, `app/chamados/services/fio.py:947`; `app/chamados/models.py:477`)

6. **Leitores não compartilham um universo único:** `fio_vivo` reduz C6 sem olhar estado da disputa; vigia delega ao mesmo juiz; reabrir a disputa muda os recortes SQL do reconciliador e do fio, sem mudar o resultado de C6 por si só. (`app/chamados/juizes.py:1242`; `app/chamados/services/vigia_fio.py:165`; `app/chamados/reconciliador.py:1038`, `app/chamados/reconciliador.py:1482`; `app/chamados/services/fio.py:214`) O fechamento equivalente localizado é `_fechar_disputa_se_quitada`, que usa C7 (`pode_fechar`); a resposta duplicada viva continua segurando esse caminho. (`app/chamados/services/materializacao.py:1224`, `app/chamados/services/materializacao.py:1238`; `app/chamados/models.py:1885`)

**R08f é especialmente estreito:** `_disputa()` cria continente sem chamado; o teste só exige destino correto e `fechada_em=None`, sem campos restantes, ocorrência, acesso por tela/API ou perguntas colaterais. (`app/chamados/tests/test_c20_fio_mudo.py:54`, `app/chamados/tests/test_c20_fio_mudo.py:220`, `app/chamados/tests/test_c20_fio_mudo.py:227`, `app/chamados/tests/test_c20_fio_mudo.py:229`) O leitor HTML `montar_chamados_fio` exclui disputas sem chamado, logo o próprio cenário não prova a promessa “colaborador alcança a pergunta” por esse leitor. (`app/chamados/services/fio.py:181`, `app/chamados/services/fio.py:183`)

## 05/11 22:48 · sessao e720195d15d6 · F-346 · verificador · origem=Code

**O delta cura estaticamente o achado (1): a troca persiste exatamente os cinco campos que altera, preservando os demais. R06/R07 têm mordida para os mutantes anunciados, mas deixam lacunas específicas.** (`app/ferias/services.py:379`, `app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/services.py:392`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`)

Executei somente a extração do AST em memória. Execução Django, originais/mutantes em banco, incidência em produção e conferência independente de hash/pai/base: **[nao verificado]**. [nao verificado]

**1. Cura, campos e chamadores**

Não ficou fora nenhum campo de AG atribuído pela troca. O ajuste de `PA.dias_concedidos/status` continua persistido separadamente. O override continua chamando guarda, persistência e sincronização; incluir as duas datas mantém habilitada a guarda de sobreposição. (`app/ferias/services.py:378`, `app/ferias/services.py:392`, `app/ferias/services.py:394`, `app/ferias/models.py:397`, `app/ferias/models.py:434`)

| Campo persistido | Quem escreve | Quem lê |
|---|---|---|
| `data_inicio` | Criações histórico/execução/aprovação; alteração pela troca. (`app/ferias/services.py:280`, `app/ferias/services.py:322`, `app/ferias/services.py:436`, `app/ferias/services.py:379`) | Guarda de edição, coerência, colisão, materialização, consultas por janela e apresentação. (`app/ferias/models.py:324`, `app/ferias/models.py:373`, `app/ferias/models.py:428`, `app/ponto/services/ausencia.py:1168`, `app/ferias/services.py:51`, `app/templates/ferias/_secao_colab.html:117`) |
| `data_fim` | Mesmas criações e troca. (`app/ferias/services.py:280`, `app/ferias/services.py:322`, `app/ferias/services.py:437`, `app/ferias/services.py:379`) | Coerência, colisão, materialização, consultas por janela e apresentação. (`app/ferias/models.py:370`, `app/ferias/models.py:429`, `app/ponto/services/ausencia.py:1168`, `app/ferias/services.py:51`, `app/templates/ferias/_secao_colab.html:118`) |
| `dias` | Mesmas criações e troca. (`app/ferias/services.py:281`, `app/ferias/services.py:323`, `app/ferias/services.py:438`, `app/ferias/services.py:379`) | Saldo para remarcar, cancelamentos, ajuste do PA, coerência, fracionamento e apresentação. (`app/ferias/models.py:222`, `app/ferias/services.py:333`, `app/ferias/services.py:353`, `app/ferias/services.py:378`, `app/ferias/models.py:374`, `app/ferias/situacao.py:122`, `app/templates/ferias/_secao_colab.html:119`) |
| `aviso_emitido` | Default; execução; aprovação; troca. (`app/ferias/models.py:305`, `app/ferias/services.py:323`, `app/ferias/services.py:439`, `app/ferias/services.py:380`) | KPI/filtro, situação, mensagem da remarcação e apresentação. (`app/ferias/views.py:26`, `app/ferias/views.py:77`, `app/ferias/situacao.py:257`, `app/ferias/views.py:671`, `app/templates/ferias/_secao_colab.html:121`) |
| `status` | Default; histórico; cancelamentos; troca; alinhamento do cron. (`app/ferias/models.py:309`, `app/ferias/services.py:282`, `app/ferias/services.py:336`, `app/ferias/services.py:361`, `app/ferias/services.py:384`, `app/ferias/services.py:518`) | Guardas, consultas por janela, fracionamento, ativação da materialização e apresentação. (`app/ferias/views.py:452`, `app/ferias/views.py:649`, `app/ferias/services.py:51`, `app/ferias/situacao.py:119`, `app/ponto/services/ausencia.py:1164`, `app/templates/ferias/_secao_colab.html:120`) |

| Campo excluído do UPDATE | Quem escreve e quem lê |
|---|---|
| `periodo_aquisitivo_id` | Preenchido nas criações; lido pela troca, guarda, saldo e materialização. A troca não o altera. (`app/ferias/services.py:279`, `app/ferias/services.py:322`, `app/ferias/services.py:435`, `app/ferias/services.py:375`, `app/ferias/models.py:354`, `app/ponto/services/ausencia.py:1163`) |
| `agendado_por_id` | Preenchido nas criações; lido como autor das ausências e no filtro do lote. (`app/ferias/services.py:283`, `app/ferias/services.py:323`, `app/ferias/services.py:440`, `app/ponto/services/ausencia.py:1189`, `app/ferias/views.py:536`) |
| `obs` | Preenchido nas criações; cancelamento admin lê, concatena e persiste o carimbo. (`app/ferias/services.py:284`, `app/ferias/services.py:323`, `app/ferias/services.py:441`, `app/ferias/services.py:360`, `app/ferias/services.py:362`) |
| `confirmacao` | Pedido e resposta escrevem; envio, resposta, atendimento e template leem. (`app/ferias/services.py:486`, `app/ferias/services.py:496`, `app/ferias/services.py:499`, `app/ferias/views.py:838`, `app/ferias/views.py:880`, `app/chamados/views.py:628`, `app/templates/ferias/_secao_colab.html:122`) |
| `confirmado_em` | Pedido limpa e confirmação carimba; teste lê o carimbo. Leitor específico de runtime: **[nao verificado]**. (`app/ferias/services.py:487`, `app/ferias/services.py:500`, `app/ferias/test_confirmacao_agendamento.py:76`) |
| `criado_em` | Nasce por `auto_now_add`; leitor específico de runtime: **[nao verificado]**. (`app/ferias/models.py:319`) |
| PK | Identifica o agendamento carregado pelas views e apresentado pela API; a troca não lhe atribui novo valor. (`app/ferias/views.py:439`, `app/ferias/views.py:638`, `app/api/views.py:2258`, `app/ferias/services.py:379`) |

Nos dois chamadores de runtime localizados, **não há dependência do save completo**: nenhum altera os campos excluídos antes da chamada. A correção prepara auditoria e depois recarrega os objetos; a remarcação lê `aviso_emitido`, ainda atribuído e persistido pela troca. (`app/ferias/views.py:439`, `app/ferias/views.py:470`, `app/ferias/views.py:490`, `app/ferias/views.py:496`, `app/ferias/views.py:638`, `app/ferias/views.py:668`, `app/ferias/views.py:671`, `app/ferias/services.py:392`)

**2. Mordida dos testes** [nao verificado]

**R06 morde a retirada isolada do `atomic`.** O DELETE precede o veto; sem o bloco do serviço, a exceção da guarda não desfaz aquele DELETE. O `TestCase` externo não mascara isso: a comparação ocorre depois de `assertRaises`, antes do encerramento do teste. A comparação de ausências discrimina o mutante; a de saldo acrescenta pouco nesse cenário, pois os dias permanecem 10 e o save do PA viria depois do veto. Essa é uma dedução estática. (`app/ferias/tests.py:19`, `app/ferias/services.py:377`, `app/ferias/services.py:378`, `app/ferias/services.py:387`, `app/ferias/services.py:394`, `app/ferias/models.py:432`, `app/ferias/test_remarcar_limpa_janela_f346.py:100`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`)

**R07 morde a volta ao save completo**, ou a inclusão indevida de `confirmacao` na lista: carrega antes, altera a coluna diretamente, remarca com o objeto antigo e recarrega. Contudo, acrescentar somente `confirmado_em` ou somente `obs` ao UPDATE passaria: o caso não modifica nem confere esses campos. (`app/ferias/test_remarcar_limpa_janela_f346.py:110`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`, `app/ferias/services.py:392`)

| Mutante | Resultado deduzido dos casos |
|---|---|
| **Save antes do DELETE**, mantendo `exceto` | **R01–R07 deixam passar.** A guarda exclui as ausências antigas; sync e limpeza posterior chegam aos mesmos resultados observados. R06 veta antes da limpeza. Os casos não certificam a ordem nem os efeitos intermediários. (`app/ferias/models.py:420`, `app/ponto/services/ausencia.py:1182`, `app/ponto/services/ausencia.py:1252`, `app/ferias/test_remarcar_limpa_janela_f346.py:55`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`) |
| **Omitir `exceto`** | R06/R07 isolados deixam passar; **R02/R03 mordem**, pois exigem preservar as PKs da interseção/borda. (`app/ferias/test_remarcar_limpa_janela_f346.py:64`, `app/ferias/test_remarcar_limpa_janela_f346.py:72`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`) |
| **Ler janela antiga depois da atribuição** | R06/R07 isolados deixam passar; **R01 morde**, pois a janela antiga permanece materializada. (`app/ferias/services.py:379`, `app/ponto/services/ausencia.py:1250`, `app/ferias/test_remarcar_limpa_janela_f346.py:56`) |
| Omitir **`data_inicio` ou `data_fim`** do UPDATE | **R01 morde**, recarregando e exigindo ambas as datas novas. (`app/ferias/test_remarcar_limpa_janela_f346.py:47`, `app/ferias/test_remarcar_limpa_janela_f346.py:55`) |
| Omitir **`dias`** | **R01–R07 deixam passar**: R04 confere ausências e saldo do PA, sem conferir `AG.dias`. Há asserção discriminante nos testes anteriores de remarcação/admin. (`app/ferias/test_remarcar_limpa_janela_f346.py:74`, `app/ferias/test_remarcar_limpa_janela_f346.py:78`, `app/ferias/test_editar_agendamento.py:59`, `app/ferias/test_editar_datas_admin.py:93`) |
| Omitir **`aviso_emitido`** | **R01–R07 deixam passar**; a mensagem usa o objeto em memória, que recebeu a atribuição mesmo sem persistência. (`app/ferias/services.py:380`, `app/ferias/views.py:671`, `app/ferias/test_remarcar_limpa_janela_f346.py:46`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`) |
| Omitir **`status`** | **R01–R07 deixam passar**: partem de `agendado` e mantêm datas futuras. O teste anterior de concluído → futuro tem asserção discriminante. (`app/ferias/test_remarcar_limpa_janela_f346.py:29`, `app/ferias/test_remarcar_limpa_janela_f346.py:32`, `app/ferias/test_remarcar_limpa_janela_f346.py:112`, `app/ferias/test_a3_situacao_de_ferias.py:578`, `app/ferias/test_a3_situacao_de_ferias.py:581`) |

**3. AST ampliado**

Executando o helper em memória sobre o arquivo atual, obtive **`['_trocar_janela']`**, sem falso positivo atual. O universo examinado continua restrito a `services.py`. (`app/ferias/test_remarcar_limpa_janela_f346.py:117`, `app/ferias/test_remarcar_limpa_janela_f346.py:121`, `app/ferias/services.py:379`)

Há um **falso negativo bastante plausível**, confirmado em memória:

```python
def novo(pa, i):
    AgendamentoFerias.objects.filter(periodo_aquisitivo=pa).update(data_inicio=i)
```

O detector retorna `[]`: a substring `periodo` aparece no texto da chamada por causa do filtro, e o selo descarta o escritor inteiro, embora o modelo seja AG. (`app/ferias/test_remarcar_limpa_janela_f346.py:144`)

Também escapam `qs.update(**campos)` com dicionário preparado antes e `setattr(ag, campo, valor)` com nome de campo variável: o detector exige nomes/constantes presentes na própria chamada. (`app/ferias/test_remarcar_limpa_janela_f346.py:141`, `app/ferias/test_remarcar_limpa_janela_f346.py:145`)

Como falso positivo sintético, `ag.data_inicio: int`, sem atribuição, é contado como escritor. O ramo `AnnAssign` não exige valor e não recebeu uma sentinela sintética correspondente no teste de mordida. (`app/ferias/test_remarcar_limpa_janela_f346.py:136`, `app/ferias/test_remarcar_limpa_janela_f346.py:172`)

Portanto, o selo ampliou a cobertura sintática, mas continua verificando contagem e presença nominal de chamadas; não prova transação, ordem, argumentos da limpeza ou persistência dos cinco campos. (`app/ferias/test_remarcar_limpa_janela_f346.py:162`, `app/ferias/test_remarcar_limpa_janela_f346.py:167`)

**4. Triagem do achado (2)**

**Na cópia atual, o padrão é compartilhado pelas duas portas:** ambas recebem objeto carregado na view e delegam à função que captura a janela antiga antes do `atomic`, sem recarga protegida. (`app/ferias/views.py:439`, `app/ferias/views.py:638`, `app/ferias/services.py:376`, `app/ferias/services.py:377`, `app/ferias/services.py:400`, `app/ferias/services.py:407`)

A docstring declara que a ordem já existia na correção anterior; o anexo v1 também sustenta essa origem. **O corpo anterior à obra, no commit base, não pôde ser conferido independentemente: [nao verificado].** O comentário atual é evidência documental, não substitui a leitura histórica. (`app/ferias/services.py:369`)

Recarregar simplesmente dentro do `atomic` **não elimina a corrida**: ainda permite duas leituras da mesma janela antes das escritas. A leitura feita pela guarda tampouco atualiza `_ini_ant/_fim_ant`, usados na limpeza. (`app/ferias/services.py:376`, `app/ferias/services.py:387`, `app/ferias/models.py:400`, `app/ferias/models.py:420`)

Não sustentaria a formulação absoluta “exige `select_for_update`”. Uma alternativa de desenho seria UPDATE condicionado à janela/versão esperada, com aborto de todo o `atomic` se houver conflito, preservando guarda e sync. Isso evita necessariamente acrescentar um SELECT com trava, mas continua envolvendo a trava inerente ao UPDATE e exige resolver sua integração com o chokepoint atual. **Alternativa não implementada nem validada nesta cópia: [nao verificado].** (`app/ferias/models.py:434`, `app/ferias/services.py:377`, `app/ferias/services.py:392`)

Assim, **manter a concorrência em fila de desenho é defensável**, mas a exclusividade de `select_for_update` como cura não está demonstrada. Já há ordens opostas no código: troca faz Ausência → AG → PA; cancelamento admin começa PA → Ausência → AG. (`app/ferias/services.py:387`, `app/ferias/services.py:392`, `app/ferias/services.py:394`, `app/ferias/services.py:355`, `app/ferias/services.py:356`, `app/ferias/services.py:362`)

**5. Classes novas no delta**

| Classe | Avaliação |
|---|---|
| **2º juiz** | Não identificado: permanecem os juízes de status, aviso e colisão. (`app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/services.py:393`, `app/ferias/models.py:428`) |
| **2º escritor fora da porta** | Não identificado no produto; o delta restringe a escrita dentro da porta registrada. (`app/ferias/services.py:392`, `app/core/portas.py:488`) |
| **Relógio fora da borda** | Permanece, sem leitura nova neste delta: serviço e juízes consultam o relógio separadamente. (`app/ferias/services.py:380`, `app/ferias/models.py:280`, `app/ferias/models.py:94`) |
| **Derivação paralela** | Não identificada: continuam as mesmas derivações e autoridades. (`app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/models.py:212`) |
| **Selo sem mordida** | Os novos casos mordem seus alvos anunciados; permanecem as lacunas de campos e do AST descritas acima. (`app/ferias/test_remarcar_limpa_janela_f346.py:103`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`, `app/ferias/test_remarcar_limpa_janela_f346.py:144`) |
| **Ordem de trava** | Nenhuma nova ordem explícita introduzida neste delta: limpeza → save/sync → PA permanece. A questão concorrente continua aberta. (`app/ferias/services.py:387`, `app/ferias/services.py:392`, `app/ferias/services.py:394`, `app/ferias/models.py:434`) |
| **Pouso antes de pré-requisito** | Não aparece nova dependência de schema: os cinco campos já existem. Migrações aplicadas, suite e condições de deploy: **[nao verificado]**. (`app/ferias/models.py:302`, `app/ferias/models.py:305`, `app/ferias/models.py:309`) |

## 05/11 22:46 · sessao 49ee82a1baf3 · - · - · origem=Ronald (tmux)

**R06/R07 têm mordida estática para os mutantes anunciados; execução Django/banco dos originais e mutantes: [nao verificado].** R06 chama o serviço diretamente, provoca colisão materializada e compara ausências/saldo/janela após a exceção; o veto levanta antes do `super().save()`. Sem o `atomic` do serviço, o DELETE anterior ao veto não é desfeito apenas por `assertRaises`; o `TestCase` externo permanece aberto durante a comparação. (`app/ferias/test_remarcar_limpa_janela_f346.py:87`, `app/ferias/test_remarcar_limpa_janela_f346.py:98`, `app/ferias/test_remarcar_limpa_janela_f346.py:100`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`, `app/ferias/tests.py:19`, `app/ferias/services.py:377`, `app/ferias/services.py:387`, `app/ferias/models.py:428`, `app/ferias/models.py:434`)

R07 carrega o objeto com confirmação antiga, faz `QuerySet.update(confirmacao='confirmado')`, remarca com o objeto antigo e recarrega a linha. Voltar ao save completo, ou recolocar `confirmacao` no `update_fields`, regrava o valor antigo e contraria a asserção. (`app/ferias/test_remarcar_limpa_janela_f346.py:107`, `app/ferias/test_remarcar_limpa_janela_f346.py:110`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`, `app/ferias/models.py:314`, `app/ferias/services.py:392`)

R07 **não** verifica concorrência sobre `confirmado_em` ou `obs`: esses campos não são alterados entre carga e remarcação nem conferidos depois. Um mutante que acrescentasse somente `confirmado_em` ou somente `obs` ao `update_fields` preservaria a asserção atual. (`app/ferias/test_remarcar_limpa_janela_f346.py:110`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`, `app/ferias/services.py:392`)

| Mutante | Mordida estática dos casos F-346 |
|---|---|
| Save antes do DELETE, mantendo `exceto` e `atomic` | **Passa pelos resultados observados.** A guarda exclui as ausências da janela antiga; o sync preserva interseção/cria dias novos; o DELETE posterior com `exceto` retira apenas antiga − nova. R06 veta antes de alterar e igualmente preserva o estado. Isso muda ordem/efeitos intermediários, mas esses casos conferem o resultado final, não a ordem. (`app/ferias/models.py:420`, `app/ferias/models.py:428`, `app/ponto/services/ausencia.py:1182`, `app/ponto/services/ausencia.py:1252`, `app/ferias/test_remarcar_limpa_janela_f346.py:55`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`) |
| Omitir `exceto` | **R02/R03 mordem:** interseção e dia de borda são apagados e recriados com outra PK. (`app/ferias/test_remarcar_limpa_janela_f346.py:61`, `app/ferias/test_remarcar_limpa_janela_f346.py:64`, `app/ferias/test_remarcar_limpa_janela_f346.py:69`, `app/ferias/test_remarcar_limpa_janela_f346.py:72`, `app/ponto/services/ausencia.py:1252`) |
| Capturar antiga depois da atribuição | **R01 morde:** o helper passa a receber a própria janela nova, inteiramente excluída por `exceto`, deixando a antiga materializada. (`app/ferias/services.py:376`, `app/ferias/services.py:379`, `app/ferias/services.py:387`, `app/ponto/services/ausencia.py:1250`, `app/ferias/test_remarcar_limpa_janela_f346.py:56`) |
| Omitir `data_inicio` ou `data_fim` do save | **R01 morde:** recarrega e exige ambas as datas novas. (`app/ferias/test_remarcar_limpa_janela_f346.py:47`, `app/ferias/test_remarcar_limpa_janela_f346.py:55`) |
| Omitir `dias` | **R01–R07 deixam passar:** R04 muda 10→5, mas só confere ausências e `PA.dias_concedidos`; sync usa datas, sem ler `ag.dias`. Existe mordida fora desse arquivo: `test_editar_reajusta_saldo_do_periodo` e teste admin conferem `ag.dias` após redução. (`app/ferias/test_remarcar_limpa_janela_f346.py:74`, `app/ferias/test_remarcar_limpa_janela_f346.py:78`, `app/ponto/services/ausencia.py:1178`, `app/ferias/test_editar_agendamento.py:52`, `app/ferias/test_editar_agendamento.py:59`, `app/ferias/test_editar_datas_admin.py:88`, `app/ferias/test_editar_datas_admin.py:93`) |
| Omitir `aviso_emitido` | **R01–R07 deixam passar:** não o conferem; a view usa o objeto em memória, já atribuído pelo serviço. (`app/ferias/test_remarcar_limpa_janela_f346.py:49`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`, `app/ferias/services.py:380`, `app/ferias/views.py:671`) |
| Omitir `status` | **R01–R07 deixam passar:** todos partem de `agendado` e movem para futuro. Há mordida fora deles na correção de concluído para futuro. (`app/ferias/test_remarcar_limpa_janela_f346.py:23`, `app/ferias/test_remarcar_limpa_janela_f346.py:29`, `app/ferias/test_remarcar_limpa_janela_f346.py:32`, `app/ferias/test_remarcar_limpa_janela_f346.py:112`, `app/ferias/test_a3_situacao_de_ferias.py:571`, `app/ferias/test_a3_situacao_de_ferias.py:581`) |

**AST:** extraí `trocadores_de_janela`/`chamadas` e executei exclusivamente em memória com `python3 -B`, sem importar Django nem escrever arquivos: resultado atual `['_trocar_janela']`; portanto, não houve falso positivo sobre o `services.py` atual. O detector olha apenas esse arquivo e exclui literalmente `self`/`periodo`; a única atribuição de AG encontrada pelo detector é a de `_trocar_janela`. (`app/ferias/test_remarcar_limpa_janela_f346.py:117`, `app/ferias/test_remarcar_limpa_janela_f346.py:121`, `app/ferias/test_remarcar_limpa_janela_f346.py:134`, `app/ferias/services.py:379`)

Contraexemplos sintéticos também executados em memória:

```python
def x(qs, i):
    campos = {"data_inicio": i}
    qs.update(**campos)            # resultado []

def x(ag, i):
    campo = "data_inicio"
    setattr(ag, campo, i)          # resultado []

def x(ag_por_periodo, i):
    ag_por_periodo.update(data_inicio=i)  # resultado []
```

Os dois primeiros escapam porque o selo exige keyword explícita ou constante dentro da própria chamada; o terceiro escapa porque a exclusão testa a substring `periodo` em todo o nome da função chamada, sem identificar o modelo. Alias de `setattr`, atribuição a lista (`[ag.data_inicio, ag.data_fim] = ...`), alvo `self` e `AsyncFunctionDef` também escaparam nos exemplos executados. (`app/ferias/test_remarcar_limpa_janela_f346.py:126`, `app/ferias/test_remarcar_limpa_janela_f346.py:132`, `app/ferias/test_remarcar_limpa_janela_f346.py:134`, `app/ferias/test_remarcar_limpa_janela_f346.py:141`, `app/ferias/test_remarcar_limpa_janela_f346.py:144`)

Há falsos positivos **plausíveis**, não presentes no arquivo atual: `ag.data_inicio: int` sem valor é contado como escritor embora não atribua; qualquer outro tipo com `.data_inicio` também é contado, pois o selo não resolve o tipo do receptor. Ambos foram confirmados nos exemplos em memória. (`app/ferias/test_remarcar_limpa_janela_f346.py:133`, `app/ferias/test_remarcar_limpa_janela_f346.py:136`)

O selo estrutural tampouco prova `atomic`, ordem, parâmetros de DELETE ou persistência: exige um escritor, presença nominal da chamada de limpeza e delegação das duas portas. Sua ampliação tem mordida para as formas adicionadas, mas não constitui garantia semântica. (`app/ferias/test_remarcar_limpa_janela_f346.py:162`, `app/ferias/test_remarcar_limpa_janela_f346.py:165`, `app/ferias/test_remarcar_limpa_janela_f346.py:167`, `app/ferias/test_remarcar_limpa_janela_f346.py:177`)

## 05/11 22:45 · sessao a914ed5cd748 · - · - · origem=Ronald (tmux)

**O delta cura estaticamente o achado (1): os cinco campos de AG atribuídos pela troca são exatamente os cinco de `update_fields`; o PA continua salvo separadamente. A guarda e a sincronização continuam executadas pelo override.** (`app/ferias/services.py:378`, `app/ferias/services.py:379`, `app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/services.py:392`, `app/ferias/services.py:394`, `app/ferias/models.py:434`)

| Campo AG | Quem escreve | Quem lê |
|---|---|---|
| `data_inicio` | Criações histórico/execução/aprovação; alteração existente pela troca. (`app/ferias/services.py:280`, `app/ferias/services.py:322`, `app/ferias/services.py:436`, `app/ferias/services.py:379`) | Guarda de edição; coerência; colisão; materialização; API/tabela. (`app/ferias/models.py:324`, `app/ferias/models.py:373`, `app/ferias/models.py:428`, `app/ponto/services/ausencia.py:1168`, `app/api/views.py:2259`, `app/templates/ferias/_tabela_ferias.html:39`) |
| `data_fim` | Mesmas criações e troca. (`app/ferias/services.py:280`, `app/ferias/services.py:322`, `app/ferias/services.py:437`, `app/ferias/services.py:379`) | Coerência; colisão; materialização; API/tabela. (`app/ferias/models.py:370`, `app/ferias/models.py:429`, `app/ponto/services/ausencia.py:1168`, `app/api/views.py:2260`, `app/templates/ferias/_tabela_ferias.html:40`) |
| `dias` | Mesmas criações e troca. (`app/ferias/services.py:281`, `app/ferias/services.py:323`, `app/ferias/services.py:438`, `app/ferias/services.py:379`) | Saldo de remarcação; cancelamento; ajuste do PA; fracionamento; coerência; API. (`app/ferias/models.py:222`, `app/ferias/services.py:333`, `app/ferias/services.py:378`, `app/ferias/situacao.py:122`, `app/ferias/models.py:374`, `app/api/views.py:2261`) |
| `aviso_emitido` | Default; execução; aprovação; troca. (`app/ferias/models.py:305`, `app/ferias/services.py:323`, `app/ferias/services.py:439`, `app/ferias/services.py:380`) | KPI/filtro; situação; resposta da remarcação; tabela. (`app/ferias/views.py:26`, `app/ferias/views.py:77`, `app/ferias/situacao.py:257`, `app/ferias/views.py:671`, `app/templates/ferias/_tabela_ferias.html:43`) |
| `status` | Default; histórico; cancelamentos; troca; cron. (`app/ferias/models.py:309`, `app/ferias/services.py:282`, `app/ferias/services.py:336`, `app/ferias/services.py:361`, `app/ferias/services.py:384`, `app/ferias/services.py:518`) | Guardas de portas; frações; guarda de sobreposição; ativação da materialização; API. (`app/ferias/views.py:452`, `app/ferias/views.py:649`, `app/ferias/situacao.py:119`, `app/ferias/models.py:394`, `app/ponto/services/ausencia.py:1164`, `app/api/views.py:2262`) |

| Campo fora do UPDATE | Quem escreve e quem lê |
|---|---|
| `periodo_aquisitivo_id` | Criações preenchem; troca não altera. Guarda, saldo, sync e tabela leem a relação. (`app/ferias/services.py:279`, `app/ferias/services.py:322`, `app/ferias/services.py:435`, `app/ferias/services.py:375`, `app/ferias/models.py:354`, `app/ferias/models.py:428`, `app/ponto/services/ausencia.py:1163`, `app/templates/ferias/_tabela_ferias.html:36`) |
| `agendado_por_id` | Criações preenchem; sync usa como autor da ausência e lote filtra por ele. (`app/ferias/services.py:283`, `app/ferias/services.py:323`, `app/ferias/services.py:440`, `app/ponto/services/ausencia.py:1189`, `app/ferias/views.py:536`) |
| `obs` | Criações preenchem; cancelamento admin lê, concatena carimbo e salva. (`app/ferias/services.py:284`, `app/ferias/services.py:323`, `app/ferias/services.py:441`, `app/ferias/services.py:360`, `app/ferias/services.py:362`) |
| `confirmacao` | Pedido e resposta escrevem; envio, resposta, atendimento e template leem. (`app/ferias/services.py:486`, `app/ferias/services.py:496`, `app/ferias/services.py:499`, `app/ferias/views.py:838`, `app/ferias/views.py:880`, `app/chamados/views.py:628`, `app/templates/ferias/_secao_colab.html:122`) |
| `confirmado_em` | Pedido limpa; confirmação carimba. Leitor de runtime específico não localizado: **[nao verificado]**; teste lê o carimbo. (`app/ferias/services.py:487`, `app/ferias/services.py:500`, `app/ferias/test_confirmacao_agendamento.py:76`) |
| `criado_em` | `auto_now_add`; leitor específico de runtime para AG não localizado: **[nao verificado]**. (`app/ferias/models.py:319`) |
| PK | Identifica os objetos usados pelas portas/views e aparece na API; necessidade de regravá-la pela troca não demonstrada. (`app/ferias/views.py:439`, `app/ferias/views.py:638`, `app/api/views.py:2258`) |

**Nenhum dos dois chamadores de runtime localizados depende de persistir outros campos de AG.** A correção carrega o objeto, valida/consulta saldo, prepara auditoria e chama o serviço; depois recarrega os objetos. A remarcação carrega, valida datas/saldo e chama o serviço; depois lê `aviso_emitido`, que continua atribuído e persistido. (`app/ferias/views.py:439`, `app/ferias/views.py:465`, `app/ferias/views.py:470`, `app/ferias/views.py:490`, `app/ferias/views.py:496`, `app/ferias/views.py:638`, `app/ferias/views.py:661`, `app/ferias/views.py:668`, `app/ferias/views.py:671`)

O save limitado ainda inclui ambas as datas; portanto não aciona o retorno antecipado reservado a updates sem datas. O override chama guarda, `super().save` e sync na mesma sequência. (`app/ferias/services.py:392`, `app/ferias/models.py:397`, `app/ferias/models.py:434`)

Execução de testes, SQL efetivo e cobertura de caminhos dinâmicos/externos: **[nao verificado]**. [nao verificado]

## 05/11 22:27 · sessao e05481d49698 · H12H06-B-SIMPLES · sonda · origem=Code

Saída fornecida, transcrita literalmente; não houve nova execução da sonda. Execução e hash informados: [não verificado].

```text
sonda_leitura: sonda_h12_comp10.py [8d165ce05418159e] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

emp2 janela=2026-09-21..2026-10-20 entregue=False exp=None hash=None fm=477 demitidos=71 demissao_apos_fim=12 no_txt=0 ids=col343,col574,col878,col981,col518,col318,col912,col663,col973,col270,col699,col907
emp3 janela=2026-09-21..2026-10-20 entregue=False exp=None hash=None fm=119 demitidos=18 demissao_apos_fim=7 no_txt=0 ids=col761,col79,col90,col752,col984,col755,col641
emp4 janela=2026-09-21..2026-10-20 entregue=False exp=None hash=None fm=24 demitidos=0 demissao_apos_fim=0 no_txt=0 ids=
```

Conforme exclusivamente o dado raspado fornecido, sem validação independente: [não verificado].

| Empresa | 10/2026 entregue, vigente não invalidada? | Colaboradores do FM com demissão após 20/10 | Destes, com linha no TXT vigente | Fonte |
|---|---|---:|---:|---|
| 2 | Não (`entregue=False`, `exp=None`) | 12 | 0 | Dado fornecido [não verificado] |
| 3 | Não (`entregue=False`, `exp=None`) | 7 | 0 | Dado fornecido [não verificado] |
| 4 | Não (`entregue=False`, `exp=None`) | 0 | 0 | Dado fornecido [não verificado] |

O código define entrega pela existência de exportação vigente com `invalidada_em is None`. [app/folha/export.py:580](app/folha/export.py:580) [app/folha/export.py:583](app/folha/export.py:583)

Na comparação solicitada:

- **Antes do POUSO A:** o registro da alteração descreve o predicado anterior como `if colab.data_demissao is not None:`, sem consultar a janela; esses colaboradores seriam excluídos como rescisão. [app/docs/RELATO.md:90](app/docs/RELATO.md:90) [app/folha/tests/test_rescisao_modulo_proprio.py:44](app/folha/tests/test_rescisao_modulo_proprio.py:44)
- **Atual, na cópia:** `sai_pela_rescisao` exige demissão até o fim da janela, inclusive. Demissão posterior deixa de causar `fora / rescisao_modulo_proprio` e segue para os demais filtros. [app/folha/export.py:60](app/folha/export.py:60) [app/folha/export.py:858](app/folha/export.py:858) [app/folha/export.py:864](app/folha/export.py:864)

Portanto, numa nova geração, **os 12 da empresa 2 e os 7 da empresa 3 deixariam de ser excluídos por esse motivo**; na empresa 4, o grupo informado é vazio. Quantidades: dado fornecido [não verificado]; efeito do predicado: [app/folha/export.py:60](app/folha/export.py:60) [app/folha/export.py:858](app/folha/export.py:858).

Isso não demonstra que ganhariam linhas: ainda existem filtros de código, férias, cadastro, espelho e homologação, além das condições da montagem. **Quantos efetivamente entrariam, com quais rubricas e valores: [não verificado].** [app/folha/export.py:760](app/folha/export.py:760) [app/folha/export.py:960](app/folha/export.py:960) [app/folha/export.py:708](app/folha/export.py:708) [app/folha/export.py:725](app/folha/export.py:725)

Identidade da cópia com o HEAD solicitado: [não verificado].

## 05/11 22:20 · sessao 1640fe6c71e4 · PD-H12H06-B · censo · origem=Code

**O ponto decisivo: reexecutar `save()` sobre `minutos=720` preserva os 720. A derivação automática existe apenas para `falta`, aprovada, com minutos zero; não existe para atestado.** `app/ponto/models.py:877-887`.

**Também há impacto além da duração:** atestado de um dia passando de 720 para menos de 480 pode virar cobertura parcial, conforme o cadastro vivo do tipo. `app/ponto/turnos.py:2580-2600`.

**1. Escritores e possibilidade de substituir o positivo**

| Escritor | Decisão relevante |
|---|---|
| **`Ausencia.save()`** | Aceita um novo positivo atribuído pelo chamador e o grava; não recalcula positivo nem deriva atestado. Só calcula falta aprovada com minutos falsy. `app/ponto/models.py:877-887,908`. |
| **`criar_ausencia()`** | Valida minutos contra `parciais_vigentes()`, salvo `checar_parcial=False`; cria e chama `save`. Nos modos de devolver existente, retorna a linha sem corrigir minutos. `app/ponto/services/ausencia.py:206-209,246-252,294-314,385,417-418`. |
| **`garantir_ausencia()`** | Usa `se_existir='retornar'`: existente fica intacta. `app/ponto/services/ausencia.py:453-462,311-314`. |
| **`editar_ausencia_registro()`** | Substitui positivo, mas recusa minutos truthy se o tipo não estiver em `parciais_vigentes()`. Com `None/0`, zera; somente falta aprovada será derivada pelo modelo. Também revalida sobreposição e reescreve datas/range. `app/ponto/services/ausencia.py:473-508`. |
| **`mudar_estado()`** | Mesmo estado retorna sem salvar. Mudando estado, inclui `minutos` em `update_fields`, mas não recebe um novo valor nem zera o anterior; portanto conserva 720. `app/ponto/services/ausencia.py:801-819`. |
| **`decidir()`** | Chama `mudar_estado`, altera carimbos, descrição e período; não recebe minutos. Não é uma porta de recomposição da duração. `app/ponto/services/ausencia.py:881-911`. |
| **Materialização** | Hoje omite minutos: falta aprovada deriva; atestado nasce com zero, documentado como dia inteiro. Existente pode voltar sem atualização. `app/chamados/services/materializacao.py:484-492,519-528`. |
| **`rederivar_fim()`** | Apenas `save()` e `refresh_from_db()`; não corrige 720 positivo. `app/ponto/services/ausencia.py:1277-1282`; `app/ponto/models.py:877`. |
| **Espelhamento de férias** | Cria com minutos zero; ignora dia existente. `app/ponto/services/ausencia.py:1182-1191`. |
| **Django admin** | `AusenciaAdmin` é somente leitura; não permite adicionar, alterar ou excluir. `app/ponto/admin.py:37-48`; `app/core/admin.py:59-75`. |

Os trechos que decidem são:

```python
if self.status == 'aprovada' and self.tipo == 'falta' and not self.minutos:
    ...
    self.minutos = _tot
...
super().save(*args, **kwargs)
```

`app/ponto/models.py:877-908`.

```python
if minutos and tipo not in _CATV.parciais_vigentes():
    raise AusenciaInvalida(..., codigo='tipo_nao_parcial')
...
ausencia.minutos = int(minutos or 0)
ausencia.save()
```

`app/ponto/services/ausencia.py:480-508`.

Portanto, **o modelo aceita gravar o previsto calculado externamente sobre um positivo**, desde que `minutos` entre nos campos efetivamente salvos; a edição de serviço só aceita esse positivo conforme o cadastro parcial do tipo e ainda altera/revalida outros campos. `app/ponto/models.py:877-908`; `app/ponto/services/ausencia.py:473-508`.

A permissão para lançar por minutos vem do cadastro vivo; sua situação nas 30 linhas não foi consultada. `app/ponto/catalogo/ausencias.py:336-358`; **[não verificado]**.

**2. Efeitos, gravações e travas**

A sequência verificável do caminho direto é esta; ela contém ramos condicionais, não uma lista fixa de entidades sempre escritas. `app/ponto/signals.py:85-93,143-147`; `app/ponto/services/cartorio.py:1358-1367,1488-1522,1799-1823`.

1. **Modelo → Ausencia:** carimba decisão sem `aprovada_em`, eventualmente deriva minutos, normaliza o range e chama `super().save()`. Não há `select_for_update` explícito nesse método. `app/ponto/models.py:868-908`.
2. **`post_save` → cartório, imediatamente:** salvo `raw`, chama `_julgar_dias`; o antirrecursão pode interromper. Percorre os dias da ausência, podendo alcançar várias células de um range mesmo que o comando salve uma única ausência. `app/ponto/signals.py:85-93,115-128,143-147`.
3. **Cartório → leitura da célula:** usa `.filter(...).first()`, sem trava explícita nessa busca; célula inexistente/futura ou turno corrente ainda não encerrado pode impedir o julgamento. `app/ponto/services/cartorio.py:1974-1987`.
4. **Cartório → efeitos condicionais:** pode reconciliar chamados antes de lavrar a célula, emitir cobrança e ressuscitar perguntas; depois pode reconciliar novamente e fabricar perguntas/disputas. Logo, “Ausencia → Celula e acabou” seria uma descrição incompleta. `app/ponto/services/cartorio.py:1488-1522,1600-1622,1777-1789,958-973,981-999`.
5. **Lavra → LogAuditoria → CelulaDia → EventoCelula:** no ramo normal, `lavrar_veredito(save=False)` prepara a célula e registra a trilha; `carimbar` efetiva o save. `salvar_e_emitir` grava primeiro a célula e depois insere o evento, no mesmo `atomic`. `app/ponto/services/cartorio.py:1800-1823`; `app/ponto/portas/celula.py:814-849,676-684`; `app/core/observ.py:133-138`.
6. **Precedência:** depois do julgamento, pode salvar novamente a ata da célula e emitir outro evento; igualdade evita essa escrita. `app/ponto/services/cartorio.py:1995-1997`; `app/ponto/portas/celula.py:853-864`.
7. **Consumidor, após commit:** agenda a devolução de perguntas absorvidas quando rejeitada; depois reconcilia chamados do colaborador e fecha chamados da ausência decidida. Com `atomic` externo, isso ocorre ao sair dele; não é um job enfileirado por esse código. `app/ponto/signals.py:250-279`; `app/chamados/services/silencio_ausencia.py:119-129`.

Nos ramos de chamado, **reconciliar abre `atomic`, trava o chamado com `select_for_update`, decide e grava**; a atualização do chamado vem antes da resolução/relavra das perguntas pelos receivers. Já o nascimento por `abrir` trava o **Colaborador** antes do dedup/criação. `app/chamados/models.py:616-620,638,668-675,261-279,863-867`; `app/chamados/signals.py:75-94`.

**`invalidar_fechamento` não é efeito automático de todo `Ausencia.save()`.** É uma chamada explícita de `decidir`, depois dos saves da decisão; ela percorre as competências afetadas e atualiza `FechamentoMensal.derivado_em=None`, com savepoint por competência e sem recalcular o motor. `app/ponto/services/ausencia.py:895-932`; `app/ponto/services/fechamento.py:1632-1640`.

Assim, um save direto dos minutos não garante atualização da projeção mensal; também não garante regravação da célula, pois o cartório pode pular por impressão igual, que não codifica o valor exato dos minutos. `app/ponto/signals.py:143-147,275-279`; `app/ponto/services/cartorio.py:452-456,498-508,1348-1364`.

**Sobre um `atomic` por linha:** ele não inverte, por si, a sequência interna acima nem acumula as 30 ausências numa transação. Entretanto, amplia a duração das travas até o commit daquela linha e adia os consumidores; portanto não equivale a dizer que todo `save()` em autocommit já possui esse mesmo escopo transacional. Essa é uma inferência da posição dos saves, atomics e callbacks. `app/ponto/models.py:908`; `app/ponto/portas/celula.py:681-684`; `app/ponto/signals.py:272`; `app/chamados/models.py:673-674`. Ausência de deadlock em concorrência: **[não verificado]**.

**3. Autoridade de competência entregue/exportada**

A autoridade é **`folha.export.competencia_entregue(empresa_id, mes, ano)`**: existe `ExportacaoDominio` vigente e esse vigente não está invalidado. Não basta existir qualquer exportação histórica. `app/folha/export.py:556-586`.

Para `(colaborador, data)`, já existe a composição:

```python
competencia_do_dia_entregue(data, colaborador.empresa)
```

Ela usa `janela_atual(data, empresa)` e consulta a competência do **mês/ano do fim da janela**, não o mês civil da data; sem data retorna `None`. `app/chamados/services/vigia_fio.py:822-844`.

As distinções são:

| Pergunta | Autoridade |
|---|---|
| Foi entregue? | `competencia_entregue`, sobre exportação vigente válida. `app/folha/export.py:580-586`. |
| Está aprovada/exportada para a guarda de batida? | `competencia_fechada`, sobre `FechamentoMensal.status`. `app/ponto/janelas.py:75-89`; `app/ponto/registro_batida.py:133-135`. |
| Está fechada para regenerar célula? | `competencia_fechada_para_celula`: entregue **ou** holerite do colaborador em lote publicado. `app/folha/export.py:589-616`. |
| O export pode passar pela conferência atual? | `folha/porta_export.py::conferir`, que verifica medição/carimbo/falhas; não responde se já foi entregue. `app/folha/porta_export.py:800-821`. |

Outros escritores já usam esse juiz: `competencias_a_lavrar` consulta `competencia_entregue`; o fechamento o consulta via `marcos_da_competencia`; a regeneração de células usa a composição mais restritiva própria da célula. `app/ponto/services/he_pendente_lavrado.py:209-225`; `app/ponto/services/fechamento.py:1513-1524`; `app/ponto/management/commands/gerar_celulas.py:126-174`.

Criar/editar ausência hoje não barra competência fechada; portanto o recorte “somente competência aberta” precisaria estar explícito no comando. `app/ponto/services/ausencia.py:226-233,485`. A abertura efetiva da competência informada: **[não verificado]**.

**4. Leitores e efeito da troca de 720 pelo previsto**

| Leitor | Regra e consequência |
|---|---|
| **Fechamento: faltas** | Ausências aprovadas dos tipos que descontam têm minutos rateados por todos os dias do registro; o fechamento soma e divide por 60. **Muda horas de falta** quando o total muda; num dia, 720→420 significa 12h→7h. `app/folha/export.py:87-92,113-122,138-143`; `app/ponto/services/fechamento.py:864-866,987`. |
| **Fechamento: pagas** | Usa `tipo in TIPOS_PAGOS and minutos`, sem teto de 480: soma crédito e pula a aplicação do abono integral por cobertura. Positivo→positivo mantém esse ramo; zero muda o ramo. `app/ponto/services/fechamento.py:665-669,684-692`. |
| **`efeitos_da_ausencia`** | Mesmo predicado pago+minutos truthy: soma `min_parcial_pagos` e não justifica/suprime integralmente por esse ramo. **Muda o crédito bruto**; positivo→positivo mantém o `continue`. `app/ponto/services/efeito_ausencia.py:43-61`. |
| **`credito_parcial`** | Crédito inteiro na data inicial; abatimento `min(atraso_do_dia, credito_do_dia)`, sem transportar sobra. **Muda crédito**, mas o atraso líquido só muda quando o novo crédito deixa de cobri-lo. `app/ponto/services/credito_parcial.py:23-25,47-58`; `app/ponto/services/fechamento.py:982-985`. |
| **`turnos.cobre_parte_do_dia`** | Exige tipo parcial vivo, registro de um dia e **`0 < minutos < 480`**. 720→420 pode virar parcial; 720→480/660 continua inteiro; 720→0 também continua inteiro; range maior que um dia não vira parcial. `app/ponto/turnos.py:2580-2600`. |
| **Dia inteiro/pontualidade** | Exclui falta e exige `not cobre_parte_do_dia`; o mapa de dias abonados inteiros alimenta o motor. **Pode mudar** ao cruzar 480, alterando a cobertura da pontualidade. `app/ponto/turnos.py:2668-2690`; `app/ponto/services/fechamento.py:680-683,742`. |
| **Grade/cobrança** | Inteira dispensa o dia; parcial preserva a cobrança restante e dispensa marcos conforme o período. **Pode reativar cobrança** ao passar para 1..479; sem horas declaradas, resulta `sem_periodo`. `app/escala/utils.py:1195-1212,1251-1274`; `app/ponto/turnos.py:2632-2641`; `app/escala/servico_jornada.py:340-345`. |
| **Abono materializado** | `por_dia_da_grade` usa previsto e realizado da grade, não diretamente `Ausencia.minutos`. **Não substitui diretamente o abono pelo novo campo**; efeitos indiretos dependem da classificação. `app/ponto/services/dia_pago.py:384-400`. |
| **TXT, falta 8792** | Expande datas e emite `len(datas)`, mais registro 11 por data. **Não muda diretamente** mantendo tipo/status/datas: conta dias, não as horas gravadas. `app/folha/export.py:157-185,215-221,732-736`. |
| **TXT, atraso 8069** | Usa atraso/saída do fechamento. **Pode mudar indiretamente** após apuração, pelo crédito ou pela cobertura. `app/folha/export.py:449-452`; `app/ponto/services/fechamento.py:982-986`. |
| **Datas abonadas/8932** | A contagem crua expande datas sem ler minutos; **não muda só pela duração**. `app/folha/export.py:193-206`. |
| **Espelho, rótulos** | Rótulo aprovado de desconto/abono apresenta horas derivadas de minutos; **muda o texto numérico**. Zero elimina a duração no rótulo. `app/ponto/services/dia_decidido.py:148-155,217-235,484-489`. |
| **Espelho, totais** | Tela/PDF recebem resumo do fechamento/lavratura; **dependem da reapuração**, não apenas do novo valor na ausência. Contagem de dias de falta continua pelas datas. `app/ponto/services/espelho.py:1283-1284`; `app/relatorios/pdf_espelho.py:628`; `app/relatorios/cartao_pela_celula.py:320-346`. |
| **API** | GET serializa `a.minutos` diretamente: **muda o JSON**. `app/api/views.py:1869-1880`. |
| **Lista web** | Mostra dias primeiro, depois minutos se `cobre_parte`, senão “1 dia”/fim aberto. **Pode mudar de “1 dia” para minutos** ao cruzar o juiz de 480. `app/ponto/views.py:2233-2243`; `app/templates/ponto/ausencias.html:159-165`. |
| **Drawer/formulário/admin** | Exibem/preenchem minutos; propriedade `horas` divide por 60. **Mudam os números apresentados**. `app/templates/ponto/partials/painel_ausencia.html:125,149,296-313`; `app/ponto/admin.py:44`; `app/ponto/models.py:864-866`. |

**Há dois conceitos distintos de parcial:** crédito/efeito usam pago+minutos truthy; cobertura usa tipo parcial+um dia+`0<minutos<480`. Não se pode interpretar “previsto correto gravado” como “mesmo efeito funcional garantido”. `app/ponto/services/credito_parcial.py:24-25`; `app/ponto/services/efeito_ausencia.py:44-52`; `app/ponto/turnos.py:2598-2600`.

A antiga comparação de apresentação `>=720` foi retirada; a view atual delega ao juiz de cobertura. `app/ponto/views.py:2233-2240`.

**5. Moldes de backfill, snapshot e reversão**

- **Molde mais próximo:** `curar_ausencia_sem_fim` identifica anomalias, oferece dry-run, passa cada linha pelo serviço e registra antes/depois. Não corrige positivo porque `rederivar_fim` só chama o save existente. `app/ponto/management/commands/curar_ausencia_sem_fim.py:29-53`; `app/ponto/services/ausencia.py:1277-1282`.
- **Backfill histórico de Ausencia:** migration 0057 muda estados e registra uma trilha por linha; possui reversa, mas ela volta estados para `pendente`, não restaura minutos nem uma fotografia exata. `app/ponto/migrations/0057_a4_backfill_pendente.py:35-65`.
- **Backfill de célula:** `backfill_b5_insumos` usa o juiz e carimba insumos na célula; não altera `Ausencia.minutos`. `app/ponto/management/commands/backfill_b5_insumos.py:51-84`.
- **Snapshot/restore reutilizável como formato:** scripts existentes fotografam/restauram `FechamentoMensal` e `DiaPago`, não Ausencia. `bin/snapshot_relavratura_10_2026.py:20-64`; `bin/restore_relavratura_10_2026.py:69-86`.
- **Trilha não equivale a snapshot garantido:** `evento` é best-effort e pode retornar `None`; inferência: não deve ser tratado sozinho como garantia de reversão completa. `app/core/observ.py:94-105,124-139`.

Não foi localizado um comando que já substitua esses 720 positivos pelo previsto, nem snapshot/restore específico de Ausencia; inexistência absoluta: **[não verificado]**.

**6. Contraexemplos e resultados zero/None**

| Caso | Consequência |
|---|---|
| **Salvar mesmo valor novamente** | Save completo continua chegando ao banco, move `atualizado_em` e dispara os receivers. A segunda execução precisa de comparação explícita para ser sem escrita; inferência desses trechos. `app/ponto/models.py:813-814,908`; `app/ponto/signals.py:143-147`. |
| **Previsto legítimo de 720** | O registro continua no filtro `minutos=720`; esse filtro sozinho não identifica pendência. Comparar atual e previsto evita regravação; inferência do cálculo. `app/escala/utils.py:2293-2341`. |
| **Insumos alterados entre execuções** | Célula, DNA, vínculo, template e folgas podem produzir outro previsto na segunda execução. `app/escala/utils.py:2280-2290`; `app/escala/models.py:1094-1096,1124-1133,1280-1288`. |
| **Zero atribuído a falta aprovada** | O save chama novamente o helper; não preserva necessariamente o zero calculado antes, se os insumos mudarem nesse intervalo. `app/ponto/models.py:877-887`. |
| **Range legado incoerente** | Calcula minutos antes de derivar `data_fim` por `dias_corridos`. Primeira passada pode calcular um dia e criar o range; uma nova derivação pode então somar vários dias. `app/ponto/models.py:882-887,893-907`. |
| **Sem escala/tipo/marcos completos** | Helper devolve **0** quando não encontra vínculo/tipo ou falta entrada/saída. `app/escala/utils.py:2277-2292`. |
| **Folga ou dia indefinido** | `eh_dia_trabalho(...) is not True` devolve **0**, inclusive quando a resposta é `None`; célula existente manda por `trabalha`. `app/escala/utils.py:2287-2288`; `app/escala/models.py:1087-1096`. |
| **Feriado** | Não é zero universal: vira folga conforme ciclo autorizado, configuração e feriado no lugar do vínculo. Célula existente continua tendo precedência. `app/escala/models.py:1094-1096,1367-1395`. |
| **12x36** | Sem foto do mês, âncora individual válida decide pela paridade; folga retorna zero. Template 12x36 sozinho pode responder `None`, que o helper converte em zero. Foto mensal pode substituir a fase da âncora. `app/escala/models.py:824-829,1272-1292,1309-1317,1351-1364`; `app/escala/utils.py:2287-2288`. |
| **Turno noturno/pausas** | Calcula span com virada de meia-noite e desconta pausas; não usa simplesmente 720 nem `minutos_jornada`. Entrada igual à saída produz 1440 antes das pausas, pela aritmética existente. `app/escala/utils.py:2293-2295,2312,2326-2341`. |

**O helper normalmente retorna inteiro, inclusive zero; não retorna `None` nos caminhos normais examinados.** O zero mistura “folga” com “não há informação suficiente de escala/marcos”, portanto não prova sozinho que o dia deveria ter duração zero. `app/escala/utils.py:2277-2292,2341`.

## 05/11 22:17 · sessao 916f5668e728 · - · - · origem=Ronald (tmux)

Leitores de `Ausencia.minutos` e impacto da troca, por leitura estática:

| Leitor | Regra decisiva | Quando 720 vira o previsto |
|---|---|---|
| **Fechamento: falta** | `minutos_de_falta_por_dia` lê ausências aprovadas dos tipos que descontam e rateia `(a.minutos or 0) / n`, usando a expansão inteira do registro como denominador; fechamento soma o mapa e divide por 60. `app/folha/export.py:87-92,113-122,138-143`; `app/ponto/services/fechamento.py:864-866,987` | **Sim**, muda `horas_falta` e o mapa por dia. Num registro de um dia, 720→420 equivale a 12h→7h; num range, muda cada parcela proporcionalmente. `app/folha/export.py:118-122`; `app/ponto/services/fechamento.py:864-866` |
| **Fechamento: ausência paga** | `if _a.tipo in Ausencia.TIPOS_PAGOS and _a.minutos:` soma crédito; mesmo predicado dá `continue` na aplicação de abono/supressão por cobertura. Não exige `<480`. `app/ponto/services/fechamento.py:665-669,684-692` | Positivo→positivo mantém o ramo “parcial”, mas reduz crédito. Atenção: `_min_parcial_pagos` só é acumulado nesse arquivo; o desconto efetivo de atraso usa `creditos_por_dia`. `app/ponto/services/fechamento.py:663-669,978-985` |
| **`efeitos_da_ausencia`** | Pago com minutos truthy: soma minutos se a data inicial estiver na janela; na cobertura, pula o abono/supressão do dia inteiro. Retorna também `dias_com_ausencia_de_dia_inteiro`. `app/ponto/services/efeito_ausencia.py:43-61` | **Sim**, diminui `min_parcial_pagos`; positivo→positivo mantém o `continue`. A classificação `dia_inteiro` pode mudar ao cruzar 480, pois vem de outro juiz. `app/ponto/services/efeito_ausencia.py:44-61`; `app/ponto/turnos.py:2580,2598-2600,2684-2690` |
| **`credito_parcial`** | Pago com minutos truthy e data inicial dentro da janela: soma integralmente em `out[a.data]`; aplica `usa=min(atraso_do_dia, crédito_do_dia)`, sem transportar sobra. `app/ponto/services/credito_parcial.py:23-26,47-58` | Crédito bruto **muda**. Atraso líquido só muda se o crédito novo deixar de cobrir o atraso daquele dia. Fechamento usa esse resultado; `DiaPago` aplica o mesmo cap. `app/ponto/services/fechamento.py:982-985`; `app/ponto/services/dia_pago.py:188-195` |
| **`turnos.cobre_parte_do_dia`** | Tipo em `parciais_vigentes`, um dia (`data_fim` nula ou igual à inicial), e **`0 < minutos < 480`**. O 480 é fixo, não vem da grade. `app/ponto/turnos.py:2580,2595-2600` | 720→420: **muda para parcial**, se o cadastro do tipo permitir e for um dia. 720→480/660: continua inteiro; 720→0: continua inteiro. Range maior que um dia continua inteiro. O catálogo é vivo. `app/ponto/turnos.py:2595-2600`; `app/ponto/catalogo/ausencias.py:336-358` |
| **Dia inteiro / pontualidade** | `ausencia_explica_o_dia_inteiro = tipo != 'falta' and not cobre_parte_do_dia`; mapa para motor só inclui efeito `ABONA`. `app/ponto/turnos.py:2668-2690` | Atestado de um dia 720→420 pode sair do mapa que explica o dia inteiro e, portanto, alterar o teto de pontualidade; fechamento entrega esse mapa ao fluxo. `app/ponto/turnos.py:2681-2690`; `app/ponto/services/fechamento.py:680-683` |
| **Grade / cobrança** | Grade pergunta `cobre_parte_do_dia`, grava `cobertura_parcial`; inteiro apaga células/orfãs, parcial preserva a pergunta e dispensa só marcos dentro do período declarado. Serviço de jornada retorna sem cobrança quando a ausência não é parcial. `app/escala/utils.py:1195-1212,1251-1274`; `app/escala/servico_jornada.py:340-345` | Cruzar de 720 para 1..479 pode **reativar cobrança do resto do dia**; sem `hora_ini/hora_fim`, período responde `sem_periodo`. `app/escala/utils.py:1251-1274`; `app/ponto/turnos.py:2632-2641`; `app/escala/servico_jornada.py:342-345` |
| **Abono materializado da grade** | `por_dia_da_grade` usa dias trabalho/ausência; `_pv` vem de `minutos_previstos`, `_coberto=min(realizado,_pv)`, abono da ausência=`_pv-_coberto`. **Não lê diretamente `Ausencia.minutos`.** `app/ponto/services/dia_pago.py:384-400` | A simples substituição do campo não troca diretamente esse número por novo valor; ele continua vindo do previsto e realizado da grade. Efeitos indiretos por mudança de classificação/julgamento dependem dos dados. `app/ponto/services/dia_pago.py:385-399`; `app/escala/utils.py:1251-1274` |
| **TXT: 8792** | Obtém datas de falta pelo catálogo e expansão; `montar_bloco_falta` emite **`len(datas)`**, com um registro 11 por data. `app/folha/export.py:157-185,215-221,732-736` | **Não muda diretamente** por 720→previsto, mantendo tipo/status/datas. A rubrica de falta aqui conta dias, não horas do campo. `app/folha/export.py:157-185,218-220` |
| **8932 / dias abonados** | `datas_abonadas` expande datas dos tipos abonados, “contagem crua”, sem ler minutos. A busca no arquivo encontrou definição e nenhum chamador vivo de `datas_abonadas` dentro dele. `app/folha/export.py:193-206` | Datas/contagem não mudam com minutos. Uso efetivo da função fora desse arquivo: **[não verificado]**. `app/folha/export.py:201-206` |
| **TXT: 8069** | Eventos usam atraso+saída do fechamento em `rubrica_atraso_parcial`; atraso do fechamento já recebe abatimento do crédito. `app/folha/export.py:449-452`; `app/ponto/services/fechamento.py:982-986` | **Pode mudar indiretamente**, quando a redução do crédito aumentar atraso líquido após apuração. `app/ponto/services/credito_parcial.py:54-58`; `app/ponto/services/fechamento.py:985`; `app/folha/export.py:451-452` |
| **Rótulos do espelho / calendário / histórico** | `rotulo_de` passa minutos a `palavra_do_dia`; desconto imprime `Falta (desconta Xh)`; abono imprime `Atestado Xh (abonado)`; rejeitada/suprimida/pendente não usam o número no rótulo. `app/ponto/services/dia_decidido.py:148-155,217-235,484-489` | **Sim** no rótulo aprovado de desconto/abono: 12h→horas do previsto. Zero elimina o número. `app/ponto/services/dia_decidido.py:219-235` |
| **Espelho: totais** | Tela e PDF aplicam `folha_manda`; por competência, lê fechamento ou lavratura. Lavratura fornece `horas_falta`, `horas_abono=minutos_abonados/60`, `dias_falta=len(datas)`; não é simples soma do campo na apresentação. `app/ponto/services/espelho.py:1283-1284`; `app/relatorios/pdf_espelho.py:628`; `app/relatorios/cartao_pela_celula.py:167-190,320-346` | Totais acompanham o fechamento/lavratura que forem recalculados; contagem de dias de falta não muda só por minutos. PDF ainda acumula bruto `min_parcial_pagos` em `horas_abono` antes da substituição pelo resumo da folha quando aplicável. `app/relatorios/pdf_espelho.py:332-336,478-479,628`; `app/relatorios/cartao_pela_celula.py:320-346` |
| **API GET ausências** | Serializa diretamente `'minutos': a.minutos` nas últimas 30 ausências. `app/api/views.py:1861-1863,1869-1880` | **Sim**, o JSON apresenta o novo número. `app/api/views.py:1878` |
| **Lista web de ausências** | View calcula `a.cobre_parte` pelo juiz de 480; template mostra dias_corridos>1 primeiro, depois minutos se parcial, senão “1 dia” ou fim aberto. `app/ponto/views.py:2233-2243`; `app/templates/ponto/ausencias.html:159-165` | 720→420 pode mudar “1 dia” para “420min” em tipo parcial de um dia. Para falta, o catálogo congelado declara parcial=False, mas o juiz consulta cadastro vivo. `app/ponto/catalogo/ausencias.py:88,336-358`; `app/ponto/turnos.py:2595-2600`; `app/templates/ponto/ausencias.html:163-165` |
| **Drawer / formulário** | Mostra minutos se truthy; input recebe valor; bloco período parcial inclui `a.minutos`. `app/templates/ponto/partials/painel_ausencia.html:125,149,296-313` | **Sim**, número exibido/preenchido muda; pode aparecer bloco parcial se cruzar o critério de 480. `app/templates/ponto/partials/painel_ausencia.html:125,149,296-313`; `app/ponto/turnos.py:2598-2600` |
| **Admin / propriedade horas / CSV lote** | Admin lista `minutos`; `Ausencia.horas=round(minutos/60,2)`; histórico transporta minutos no cartão, e CSV lote imprime `x['minutos']`. `app/ponto/admin.py:44`; `app/ponto/models.py:864-866`; `app/ponto/services/dia_decidido.py:504-512,590-613`; `app/ponto/services/atestados_lote.py:118-131` | **Sim** nos números. Dias do histórico vêm das datas ou `dias_corridos`, portanto não mudam por isso. `app/ponto/services/dia_decidido.py:608-614` |

**Ponto central:** não há uma única definição de parcial: crédito/efeito usam **pago + minutos truthy**, enquanto cobertura usa **tipo parcial vivo + um dia + 0<minutos<480**. Um atestado 720→420 altera ambos os valores e a classificação de cobertura; 720→480 altera o crédito, mas mantém cobertura de dia inteiro. `app/ponto/services/credito_parcial.py:24-25`; `app/ponto/services/efeito_ausencia.py:44-52`; `app/ponto/turnos.py:2580,2598-2600`.

**`>=720`:** no caminho de apresentação de `Ausencia`, a comparação foi removida: comentário registra a regra antiga e a view atual delega ao juiz. A comparação viva `duracao_minutos >=720` encontrada em `justificativa.py` é da duração de uma **Justificativa**, não leitura de `Ausencia.minutos`. `app/ponto/views.py:2233-2240`; `app/ponto/services/justificativa.py:156-162`.

## 05/11 22:17 · sessao 721fa8d91626 · - · - · origem=Ronald (tmux)

5. **Molde existente, mas sem backfill específico de `minutos>0` identificado.**

- `curar_ausencia_sem_fim` seleciona a anomalia (`dias_corridos>1`, fim nulo, tipo não aberto), apresenta dry-run e, no apply, passa cada ausência pelo serviço e registra antes/depois: `app/ponto/management/commands/curar_ausencia_sem_fim.py:29-53`.
- O serviço `rederivar_fim` apenas chama `ausencia.save()`, `refresh_from_db()` e retorna; portanto não corrige os 720 positivos, pois o modelo só deriva falta aprovada com minutos falsos/zero: `app/ponto/services/ausencia.py:1277-1282`; `app/ponto/models.py:877-887`.
- Há backfill histórico de **estado**, com registro por ausência em `LogAuditoria` e função reversa: `app/ponto/migrations/0057_a4_backfill_pendente.py:35-52`, `:58-65`. A reversa volta estados para `pendente`; não é snapshot/restauração exata de minutos: `app/ponto/migrations/0057_a4_backfill_pendente.py:58-65`.
- Há snapshot/restauração executável de **FechamentoMensal e DiaPago**, útil como molde de inventário/serialização/restore, mas não de Ausencia: `bin/snapshot_relavratura_10_2026.py:20-26`, `:28-64`; `bin/restore_relavratura_10_2026.py:69-86`.
- Há backfill de insumos de ausência **na CelulaDia**, dry por padrão, que chama o juiz vivo e a porta de carimbo; não altera `Ausencia.minutos`: `app/ponto/management/commands/backfill_b5_insumos.py:51-84`.
- `LogAuditoria` dispõe de JSON antes/depois e ator do sistema; `evento` grava usando atomic próprio e é best-effort, podendo retornar `None` após falha: `app/core/models.py:295-307`; `app/core/observ.py:94-105`, `:124-139`. Logo, log best-effort não é garantia de snapshot restaurável — inferência desses trechos.
- Não localizei comando/serviço que já faça exatamente a troca dos 720 positivos pelo previsto nem snapshot/restore específico de Ausencia; inexistência absoluta: **[não verificado]**.

6. **Zeros, `None` e contraexemplos de segunda execução.**

- O helper `minutos_previstos_do_dia` retorna **0**, e não `None`, quando recebe lista vazia de escalas, não encontra vínculo, falta tipo, `eh_dia_trabalho(...) is not True` ou faltam entrada/saída; no caminho restante retorna `span`: `app/escala/utils.py:2277-2295`, `:2341`.
- `eh_dia_trabalho` pode retornar `None`: a célula existente manda por `cel.trabalha`; sem célula, usa cálculo: `app/escala/models.py:1087-1096`. O helper converte também esse `None` em 0 pela condição `is not True`: `app/escala/utils.py:2287-2288`.
- **Sem escala:** `_esc_vigente_do_dia` retorna `None` sem vínculos; o helper devolve 0: `app/escala/utils.py:2208-2209`, `:2281-2283`.
- **Folga:** célula existente com `trabalha=False` vence; sem célula, foto do mês contendo a data dá `base=False`: `app/escala/models.py:1094-1096`, `:1309-1317`; helper retorna 0: `app/escala/utils.py:2287-2288`.
- **Feriado:** no cálculo sem célula, só vira folga se `base is True` e `feriado_vira_folga` disser sim; esse predicado exige ciclo weekday autorizado, `trabalha_em_feriado=False` e feriado no lugar do vínculo. Não transforma automaticamente o plantão 12x36 em folga: `app/escala/models.py:1367-1371`, `:1389-1395`.
- **12x36:** com âncora individual posterior a 1940 e sem foto do mês, trabalha quando `delta % 2 == 0`; outro dia dá False: `app/escala/models.py:1289-1292`. O template 12x36 sozinho retorna `None`, pois não fornece fase: `app/escala/models.py:824-829`. Com foto, uma folga explícita dá False; fora dela a paridade da folga mais próxima pode decidir: `app/escala/models.py:1309-1317`, `:1351-1364`.
- **Template incompleto:** sem `hi` ou `hf`, previsto=0. Com ambos, span cruza meia-noite somando 1440 se diferença ≤0; desconta pausas declaradas, com fallback ao par `hii/hfi`: `app/escala/utils.py:2290-2295`, `:2312`, `:2326-2341`. Igualdade hi==hf significa 1440 antes das pausas, não 0: inferência de `:2293-2295`.
- **Salvar igual continua sendo escrita:** `Ausencia.atualizado_em` tem `auto_now=True`; `save()` termina incondicionalmente em `super().save`; o post-save chama o cartório mesmo sem comparar alterações: `app/ponto/models.py:813-814`, `:908`; `app/ponto/signals.py:143-147`. Sem guarda de igualdade, segunda execução pode atualizar timestamp e repetir efeitos — inferência desses trechos.
- **Filtro só `minutos=720` não prova pendência:** uma jornada legítima pode resultar em 720 e continuar elegível após a primeira execução; é necessário comparar atual com previsto antes de gravar — inferência do cálculo de span em `app/escala/utils.py:2293-2341`.
- **Insumos mutáveis:** célula pode mudar `trabalha`, DNA, geradora; sem DNA de marcos, usa template vivo; vínculo e folgas também participam. Outra execução pode calcular outro previsto: `app/escala/utils.py:2280-2290`; `app/escala/models.py:1094-1096`, `:1124-1133`, `:1280-1288`. Não há garantia de estabilidade entre execuções — inferência desses leitores.
- **Zero calculado antes pode ser recalculado dentro do save:** falta aprovada com `minutos=0` entra novamente no helper, em vez de preservar necessariamente o zero previamente calculado; insumos concorrentes podem produzir outro resultado: `app/ponto/models.py:877-887`.
- **Ordem interna pode mudar o alvo na segunda execução:** o save calcula minutos usando `data_fim or data` **antes** de derivar `data_fim` a partir de `dias_corridos`. Legado com `dias_corridos>1` e fim nulo pode somar só um dia na primeira passada e passar a ter range na segunda: `app/ponto/models.py:882-887`, `:893-907`. Isso importa especialmente se cada execução zerar minutos para forçar derivação — inferência desses trechos.

## 05/11 22:17 · sessao 973d2c701257 · - · - · origem=Ronald (tmux)

1. **Escritores e possibilidade de substituir `720`**

| Caminho | Comportamento na linha existente |
|---|---|
| `Ausencia.save` | Deriva **somente falta aprovada com minutos falsy**; positivo informado é preservado e pode ser salvo diretamente. Não deriva atestado. `app/ponto/models.py:872-887,908`. |
| `criar_ausencia` | Entrada `minutos>0` recusa tipo não parcial, salvo `checar_parcial=False`; cria objeto novo com o valor e chama save. Existente pode simplesmente retornar, sem atualização. `app/ponto/services/ausencia.py:206-209,246-252,294-301,311-314,341-345,385,417-418`. |
| `garantir_ausencia` | Encaminha `se_existir='retornar'`; existente volta intacta. `app/ponto/services/ausencia.py:453-462,311-314`. |
| `editar_ausencia_registro` | Pode sobrescrever positivo, mas **valor positivo recusa tipo não parcial**; `minutos=None/0` zera e deixa save rederivar apenas falta aprovada. Ainda revalida tipo/sobreposição e reescreve datas/range. `app/ponto/services/ausencia.py:473-494,496-508`. |
| `mudar_estado` | Estado igual retorna sem gravar. Alteração inclui minutos no `update_fields`, porém não recebe valor nem zera o anterior; positivo fica. `app/ponto/services/ausencia.py:801-819`. |
| `decidir` | Chama mudar_estado e grava carimbos/descrição/período; não recebe minutos. `app/ponto/services/ausencia.py:881-911`. |
| `rederivar_fim` | Apenas `save()` + refresh; positivo anterior permanece. `app/ponto/services/ausencia.py:1277-1282`. |
| Materialização | Agora omite minutos: falta aprovada deriva da grade; atestado fica **0 = dia inteiro**. Se já existe mesmo tipo/dia, porta retorna sem atualizar. `app/chamados/services/materializacao.py:484-492,519-528`; `app/ponto/services/ausencia.py:294-301`. |
| Férias | Só cria dia ausente com minutos=0; dia existente é ignorado. `app/ponto/services/ausencia.py:1182-1191`. |
| Django admin | Somente leitura: AusenciaAdmin herda `SoLeitura`; add/change/delete False e campos readonly. `app/ponto/admin.py:37-48`; `app/core/admin.py:59-75`. |

Trechos decisivos:

```python
if self.status == 'aprovada' and self.tipo == 'falta' and not self.minutos:
    ...
    self.minutos = _tot
super().save(*args, **kwargs)
```

`app/ponto/models.py:877-908`.

```python
if minutos and tipo not in _CATV.parciais_vigentes():
    raise AusenciaInvalida(..., codigo='tipo_nao_parcial')
...
ausencia.minutos = int(minutos or 0)
ausencia.save()
```

`app/ponto/services/ausencia.py:480-508`.

Portanto, `a.minutos = previsto; a.save(update_fields=['minutos', ...])` é tecnicamente aceito pelo modelo mesmo sobre positivo; **não encontrei serviço dedicado de correção de minutos que preserve todos os outros campos e dispense a restrição de tipo parcial**. A porta de edição é o escritor disponível, mas revalida/regrava o cadastro e pode recusar faltas/atestados conforme catálogo vivo. `app/ponto/models.py:877-908`; `app/ponto/services/ausencia.py:465-515`.

A restrição parcial é de cadastro vivo: a semente declara falta e atestado não parciais, mas não prova o cadastro atual sem banco. `app/ponto/catalogo/ausencias.py:17-23,88,95`; cadastro atual [não verificado].

A view de edição registra minutos antes/depois; o serviço editar por si só não grava essa trilha de minutos. `app/ponto/views.py:2770-2771,2794-2814`; `app/ponto/services/ausencia.py:496-515`.

3. **Competência entregue/exportada**

A autoridade é **`folha.export.competencia_entregue(empresa_id, mes, ano)`**, não `folha/porta_export.py` nem `FechamentoMensal.status`: ela pede `ExportacaoDominio` vigente e exige `invalidada_em is None`. `registro=True` devolve o próprio registro. `app/folha/export.py:556-586`.

Vigente significa mais recente por `(gerado_em, pk)` em empresa/mês/ano; não é simples `ExportacaoDominio.objects.exists()`, porque vigente revogado sem substituta não entrega. `app/folha/models.py:192-225`; `app/folha/export.py:564-586`.

Para **(colaborador, data)** há composição pronta:

```python
_ini, fim = janelas.janela_atual(dia, empresa)
k = (getattr(empresa, 'pk', empresa), fim.month, fim.year)
return bool(competencia_entregue(*k))
```

`chamados.services.vigia_fio.competencia_do_dia_entregue(dia, colaborador.empresa)`; dia None retorna None. `app/chamados/services/vigia_fio.py:822-844`. Corte vem da empresa, com clamp 2..28/default21, e competência é o mês/ano do **fim** da janela. `app/ponto/janelas.py:10-33,36-63`.

Outros escritores usam:

- `competencias_a_lavrar` mantém competência anterior enquanto `competencia_entregue` for falsa. `app/ponto/services/he_pendente_lavrado.py:209-225`.
- `empresas_exportadas_no_escopo` chama `marcos_da_competencia`, cujo campo exportada chama o mesmo juiz. `app/ponto/services/fechamento.py:1480-1499,1513-1524`.
- Regeneração de célula usa autoridade **mais estrita e distinta**: `competencia_fechada_para_celula = entregue OU holerite do próprio colaborador em lote publicado`; `_competencias_exportadas` enumera e pergunta esse juiz. `app/folha/export.py:589-615`; `app/ponto/management/commands/gerar_celulas.py:126-174`.
- `competencia_fechada` lê `FechamentoMensal.status in ('aprovado','exportado')` e responde outra pergunta: guarda de competência fechada para escrita de batida. `app/ponto/janelas.py:75-89`; `app/ponto/registro_batida.py:133-135`.
- `folha/porta_export.py::conferir` confere carimbo de medição de hoje/mesma competência/zero falhas; não é autoridade para saber se já foi entregue. `app/folha/porta_export.py:800-821`.

As portas atuais de criar/editar Ausencia **não barram competência fechada**; a remoção da guarda está explícita. Logo um backfill restrito a aberta precisaria fazer esse recorte explicitamente usando a autoridade apropriada. `app/ponto/services/ausencia.py:226-233,485`.

## 05/11 22:12 · sessao 1597824c0f67 · F-346 · verificador · origem=Code

**Não daria verde nesta volta sem triagem de dois achados: a remarcação passa a poder sobrescrever confirmação concorrente, e duas remarcações com objetos carregados da mesma janela podem deixar ausências órfãs.** O primeiro decorre do `save()` completo; o segundo, da janela antiga capturada do objeto sem recarga ou trava. (`app/ferias/services.py:376`, `app/ferias/services.py:389`, `app/ferias/services.py:496`, `app/ferias/views.py:638`)

A análise abaixo é estática. Execução RED/GREEN HTTP, suite, SQL efetivo, incidência em produção e confirmação independente de hash/pai/base: **[nao verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

`JUIZES.md` é lookup e manda consultar o registro vivo. Este registra colisão, cobertura, saldo, status do período, fracionamento e aviso; `_trocar_janela` não substitui essas autoridades. (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`, `app/core/juizes.py:239`, `app/core/juizes.py:242`, `app/core/juizes.py:252`)

No censo abaixo, AG = `AgendamentoFerias`, PA = `PeriodoAquisitivo`. As portas registradas são `ferias/services.py` para AG/PA e `ponto/services/ausencia.py` para Ausência. (`app/core/portas.py:480`)

| Campo | Escritores de runtime encontrados |
|---|---|
| AG `data_inicio/data_fim/dias`, existente | `_trocar_janela`, com atribuição em `services.py:379` e persistência em `services.py:389`. As duas funções públicas delegam a ele. (`app/ferias/services.py:379`, `app/ferias/services.py:397`, `app/ferias/services.py:404`) |
| AG, criação dos intervalos | Gozo histórico, execução de agendamento e aprovação de solicitação. (`app/ferias/services.py:278`, `app/ferias/services.py:298`, `app/ferias/services.py:321`, `app/ferias/services.py:431`, `app/ferias/services.py:441`) |
| AG `aviso_emitido` | Default falso; execução comum; aprovação; troca de janela. (`app/ferias/models.py:305`, `app/ferias/services.py:314`, `app/ferias/services.py:323`, `app/ferias/services.py:436`, `app/ferias/services.py:380`) |
| AG `status` | Default `agendado`; histórico pelo juiz temporal; cancelamento DP/admin; troca pelo juiz temporal; alinhamento chamado pelo cron. (`app/ferias/models.py:309`, `app/ferias/services.py:282`, `app/ferias/services.py:336`, `app/ferias/services.py:361`, `app/ferias/services.py:384`, `app/ferias/services.py:512`, `app/ferias/management/commands/sincronizar_status_ferias.py:48`) |
| PA `dias_concedidos` | Criação com defaults; histórico; execução; cancelamento DP/admin; troca; aprovação. (`app/ferias/services.py:259`, `app/ferias/views.py:184`, `app/ferias/services.py:299`, `app/ferias/services.py:324`, `app/ferias/services.py:333`, `app/ferias/services.py:353`, `app/ferias/services.py:378`, `app/ferias/services.py:442`) |
| PA `status` | `aplicar_status` recebe o veredito de `status_de_periodo`; os atos acima o persistem. Também há sincronização, realinhamento de período existente por `update`, abono e venda. (`app/ferias/models.py:204`, `app/ferias/models.py:212`, `app/ferias/services.py:225`, `app/ferias/services.py:266`, `app/ferias/services.py:165`, `app/ferias/services.py:508`) |

O `ag.save()` completo também alcança estas colunas, além dos cinco campos pretendidos; a PK identifica a linha no UPDATE normal. Detalhe SQL da versão instalada: **[nao verificado]**. (`app/ferias/models.py:299`, `app/ferias/models.py:310`, `app/ferias/models.py:313`, `app/ferias/models.py:319`, `app/ferias/services.py:389`)

| Coluna adicional | Escritores e leitores encontrados |
|---|---|
| `periodo_aquisitivo_id` | Preenchido nas três criações; agora também regravado pela troca. Lido por saldo, guarda, materialização, consultas e apresentação. (`app/ferias/services.py:279`, `app/ferias/services.py:322`, `app/ferias/services.py:432`, `app/ferias/services.py:389`, `app/ferias/models.py:428`, `app/ponto/services/ausencia.py:1163`, `app/templates/ferias/_tabela_ferias.html:36`) |
| `agendado_por_id` | Preenchido nas criações; agora regravado pela troca. Lido para autoria das ausências e filtro do lote. (`app/ferias/services.py:283`, `app/ferias/services.py:323`, `app/ferias/services.py:437`, `app/ferias/services.py:389`, `app/ponto/services/ausencia.py:1189`, `app/ferias/views.py:536`) |
| `obs` | Preenchido nas criações; lido, concatenado e persistido pelo cancelamento admin; agora regravado pela troca sem nova atribuição. (`app/ferias/services.py:284`, `app/ferias/services.py:323`, `app/ferias/services.py:438`, `app/ferias/services.py:360`, `app/ferias/services.py:389`) |
| `confirmacao/confirmado_em` | Escritores próprios: pedido de confirmação e resposta do colaborador; agora também a troca completa. Leitores: envio, resposta, atendimento e apresentação. (`app/ferias/services.py:481`, `app/ferias/services.py:489`, `app/ferias/services.py:389`, `app/ferias/views.py:838`, `app/ferias/views.py:879`, `app/chamados/views.py:627`, `app/templates/ferias/_secao_colab.html:122`) |
| `criado_em` | Nasce por `auto_now_add`; passa pelo save completo. Não localizei leitor específico de runtime: **[nao verificado]**. (`app/ferias/models.py:319`, `app/ferias/services.py:389`) |

Leitores encontrados dos campos de AG, agrupados por caminho:

| Caminho | Campos e referências |
|---|---|
| Modelo | Datas/status nas propriedades de edição/cancelamento; datas/dias/status no `clean`; datas/status na guarda; dias no saldo para remarcar; datas/dias na representação. (`app/ferias/models.py:219`, `app/ferias/models.py:322`, `app/ferias/models.py:348`, `app/ferias/models.py:394`, `app/ferias/models.py:450`) |
| Serviços | Datas/status nas consultas de janela, gozo, divergência e inícios; datas na duplicata; dias/datas nos cancelamentos e troca. (`app/ferias/services.py:51`, `app/ferias/services.py:63`, `app/ferias/services.py:77`, `app/ferias/services.py:102`, `app/ferias/services.py:316`, `app/ferias/services.py:333`, `app/ferias/services.py:376`) |
| Views | Listagem/KPIs; frações; cancelamento/auditoria; correção/auditoria; lote; remarcação/aviso; exclusão de PA; confirmação/push. (`app/ferias/views.py:21`, `app/ferias/views.py:71`, `app/ferias/views.py:234`, `app/ferias/views.py:362`, `app/ferias/views.py:470`, `app/ferias/views.py:527`, `app/ferias/views.py:649`, `app/ferias/views.py:671`, `app/ferias/views.py:814`, `app/ferias/views.py:838`, `app/ferias/views.py:849`) |
| Situação de férias | Status/dias nas frações; datas/status nos vivos, colisão e batidas; `aviso_emitido` no aviso. (`app/ferias/situacao.py:117`, `app/ferias/situacao.py:164`, `app/ferias/situacao.py:246`, `app/ferias/situacao.py:257`, `app/ferias/situacao.py:270`, `app/ferias/situacao.py:343`) |
| Materialização | Status/datas para dias desejados; autor para criação; datas na pauta de recusa. (`app/ponto/services/ausencia.py:1163`, `app/ponto/services/ausencia.py:1178`, `app/ponto/services/ausencia.py:1189`, `app/ponto/services/ausencia.py:1235`) |
| Folha/API | Intervalo recortado na folha; datas/dias/status na API; também aviso na mensageria. (`app/folha/export.py:480`, `app/api/views.py:2252`, `app/api/views.py:2268`, `app/api/views_mensageria.py:840`) |
| Colaboradores/inteligência | Disponibilidade/retorno; gozo; início/status/dias nas consultas e apresentação. (`app/colaboradores/views.py:2843`, `app/colaboradores/views.py:2895`, `app/inteligencia/views.py:92`, `app/inteligencia/views.py:276`, `app/inteligencia/views.py:285`) |
| Ponto | Gate e mensagem de férias; universo por janela; aviso da batida. (`app/ponto/utils.py:88`, `app/ponto/utils.py:197`, `app/ponto/services/aviso_da_batida.py:73`) |
| Comandos | Cron lê datas/status; alerta de retorno lê fim/status. (`app/ferias/management/commands/sincronizar_status_ferias.py:33`, `app/ferias/management/commands/alertar_retornos.py:20`) |
| Templates | Datas/dias/status/aviso nas tabelas, saldo e seção; datas/dias no atendimento e lote. (`app/templates/ferias/_tabela_ferias.html:39`, `app/templates/ferias/saldo.html:59`, `app/templates/ferias/_secao_colab.html:117`, `app/templates/ferias/_meu_atendimento_ferias.html:6`, `app/templates/ferias/cancelar_admin_massa.html:42`) |

Os leitores encontrados de PA `dias_concedidos/status` são os juízes/properties, sincronização/divergência, operações de saldo, seleção da aprovação, listagens/KPIs, auditoria, proteção do direito, situação, relatório e templates. (`app/ferias/models.py:95`, `app/ferias/models.py:212`, `app/ferias/models.py:217`, `app/ferias/models.py:231`, `app/ferias/models.py:239`, `app/ferias/services.py:90`, `app/ferias/services.py:118`, `app/ferias/services.py:225`, `app/ferias/services.py:378`, `app/ferias/services.py:421`, `app/ferias/views.py:29`, `app/ferias/views.py:55`, `app/ferias/views.py:473`, `app/ferias/views.py:790`, `app/ferias/situacao.py:178`, `app/relatorios/services.py:285`, `app/templates/ferias/saldo.html:20`, `app/templates/ferias/_secao_colab.html:42`)

Para Ausência `ferias`, o encadeamento é:

- AG `save` → `_sync_ausencia` → `espelhar_ferias`; este lê tipo/status/datas, preserva existentes e cria faltantes pela porta genérica. Agendamento inativo apaga as existentes da janela. (`app/ferias/models.py:379`, `app/ferias/models.py:434`, `app/ponto/services/ausencia.py:1164`, `app/ponto/services/ausencia.py:1172`, `app/ponto/services/ausencia.py:1182`)
- A troca e o `delete` individual do AG chamam `apagar_ausencias_de_ferias`; o cancelamento admin primeiro rejeita por `update`. (`app/ferias/services.py:387`, `app/ferias/models.py:439`, `app/ponto/services/ausencia.py:1246`, `app/ponto/services/ausencia.py:1257`)
- Portas genéricas existentes também podem alcançar registros desse tipo: criação, edição, estado/decisão, anexo, reabertura, rederivação e exclusão de ensaio. São caminhos na mesma porta registrada. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:792`, `app/ponto/services/ausencia.py:830`, `app/ponto/services/ausencia.py:881`, `app/ponto/services/ausencia.py:1090`, `app/ponto/services/ausencia.py:1270`, `app/ponto/services/ausencia.py:1277`)
- Leitores centrais: cobertura/universos, precedência/veto, cartório, decisão do dia, espelho, escala e silêncio de chamados. (`app/ponto/turnos.py:2529`, `app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2753`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:461`, `app/ponto/services/cartorio.py:355`, `app/ponto/services/dia_decidido.py:481`, `app/ponto/services/espelho.py:582`, `app/escala/utils.py:1195`, `app/chamados/services/silencio_ausencia.py:65`)
- Outros leitores genéricos: telas/API, detalhe do colaborador, histórico/materialização, folha, relatórios, inteligência, catálogo e vigias. (`app/ponto/views.py:2182`, `app/api/views.py:1861`, `app/colaboradores/services/detalhe.py:104`, `app/chamados/services/historico.py:129`, `app/chamados/services/materializacao.py:599`, `app/folha/export.py:76`, `app/relatorios/views.py:197`, `app/inteligencia/resumo.py:39`, `app/ponto/views_tipos_ausencia.py:24`, `app/ponto/services/vigia_ausencia.py:108`, `app/ponto/services/vigia_ausencia_veredito.py:46`)
- DELETE/INSERT acionam julgamento síncrono pelos signals e consumidores depois do commit; Ausência também tem vínculo `SET_NULL` em `CelulaDia`. (`app/ponto/signals.py:143`, `app/ponto/signals.py:150`, `app/ponto/signals.py:250`, `app/ponto/signals.py:272`, `app/escala/models.py:1796`)

Migrações/defaults e fixtures são fontes distintas dos escritores de runtime; há inclusive injeção deliberada de legado ultrapassando o override nos testes. (`app/ferias/migrations/0001_initial.py:40`, `app/ferias/migrations/0003_agendamentoferias_confirmacao_and_more.py:13`, `app/ferias/tests.py:96`, `app/ferias/test_editar_agendamento.py:16`, `app/ferias/test_sobreposicao_chokepoint.py:60`)

Exaustividade para SQL externo, reflexão, caminhos dinâmicos e escritores fora da cópia: **[nao verificado]**.

**2. Classes perguntadas**

| Classe | Resultado |
|---|---|
| **(a) Segundo juiz** | **Não identificado no diff.** Status AG vem de `status_por_data`; PA de `aplicar_status/status_de_periodo`; aviso usa a constante registrada; colisão continua no veto registrado. (`app/ferias/services.py:373`, `app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/services.py:390`, `app/ferias/models.py:212`, `app/ferias/models.py:428`, `app/core/juizes.py:242`, `app/core/juizes.py:256`) |
| **(b) Segundo escritor fora da porta** | **Não introduzido nos campos perguntados.** As duas entradas delegam ao mesmo escritor; Ausência continua pela porta registrada. O save completo, contudo, amplia o conjunto de campos regravados dentro da porta. (`app/ferias/services.py:387`, `app/ferias/services.py:389`, `app/ferias/services.py:397`, `app/ferias/services.py:404`, `app/core/portas.py:485`) |
| **(c) Relógio fora da borda** | **Presente no corpo resultante:** serviço lê `date.today`, e os juízes recebem `hoje=None` e leem novamente. É padrão preservado pelo diff fornecido, não leitura nova criada pela delegação. O teste congela o relógio, sem verificar um único instante propagado da borda. (`app/ferias/services.py:380`, `app/ferias/models.py:94`, `app/ferias/models.py:280`, `app/ferias/test_remarcar_limpa_janela_f346.py:23`) |
| **(d) Derivação paralela** | **Não identificada nos corpos alterados.** Permanecem os mesmos juízes de status, saldo e aviso. (`app/ferias/services.py:378`, `app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/services.py:390`, `app/ferias/models.py:212`) |
| **(e) Selo/RED sem mordida** | **Tem mordida, mas limitada.** Os testes observam a janela órfã e a identidade da interseção; o AST morde duas atribuições explícitas, mas não certifica qualquer segundo caminho. Detalhamento abaixo. (`app/ferias/test_remarcar_limpa_janela_f346.py:52`, `app/ferias/test_remarcar_limpa_janela_f346.py:59`, `app/ferias/test_remarcar_limpa_janela_f346.py:99`, `app/ferias/test_remarcar_limpa_janela_f346.py:130`) |
| **(f) Ordem de trava** | **Muda na remarcação; não aparece par de tabelas inédito no conjunto comparado.** Há ordens opostas preexistentes, detalhadas abaixo. (`app/ferias/services.py:353`, `app/ferias/services.py:356`, `app/ferias/services.py:387`, `app/ferias/services.py:391`) |
| **(g) Pouso antes de pré-requisito** | O corpo usa campos e portas existentes; não evidencia nova dependência de schema. Aplicação das migrações existentes, deploy, suite e estado real do dado: **[nao verificado]**. (`app/ferias/models.py:299`, `app/ferias/migrations/0003_agendamentoferias_confirmacao_and_more.py:13`, `app/ferias/services.py:374`) |
| **(h) Mudança de comportamento** | **Sim na remarcação:** limpeza da janela anterior, respectivos efeitos de DELETE e expansão do save. Na correção, o corpo fornecido foi mantido sob outro nome. Os únicos chamadores `.py` localizados são as duas views. (`app/ferias/services.py:387`, `app/ferias/services.py:389`, `app/ferias/services.py:397`, `app/ferias/services.py:404`, `app/ponto/signals.py:150`, `app/ferias/views.py:490`, `app/ferias/views.py:668`) |

O save antigo com `update_fields` **já disparava guarda e sync**: ambos são incondicionais no override. O ganho não depende de usar save completo; a mudança relevante é chamar a limpeza e ampliar as colunas persistidas. (`app/ferias/models.py:434`, `app/ferias/models.py:437`, `app/ferias/services.py:387`, `app/ferias/services.py:389`)

O AST só lê `services.py`, reconhece `Assign/AugAssign` em atributos e exclui receptores chamados literalmente `self` ou `periodo`. Extraindo seus helpers em memória, um segundo escritor por atribuição explícita foi detectado; `setattr`, `QuerySet.update` e `AnnAssign` passaram despercebidos. Escritor em outro arquivo também fica fora do universo. (`app/ferias/test_remarcar_limpa_janela_f346.py:87`, `app/ferias/test_remarcar_limpa_janela_f346.py:95`, `app/ferias/test_remarcar_limpa_janela_f346.py:99`, `app/ferias/test_remarcar_limpa_janela_f346.py:104`)

Mutante plausível não distinguido pelos testes novos: **retirar o `atomic`, mantendo limpeza e saves**. Todos os cenários funcionais novos são de sucesso; o AST não cobra transação. Um veto A2 depois do DELETE distinguiria esse mutante, verificando restauração das ausências e do PA. A versão atual contém essa proteção; passagem efetiva do mutante pela suite: **[nao verificado]**. (`app/ferias/services.py:377`, `app/ferias/services.py:387`, `app/ferias/test_remarcar_limpa_janela_f346.py:52`, `app/ferias/test_remarcar_limpa_janela_f346.py:120`)

As ordens de escrita das tabelas do domínio são:

| Caminho | Ordem |
|---|---|
| Remarcar antes | AG U → Ausência I pelo sync → PA U, conforme diff fornecido; arquivo anterior independente: **[nao verificado]**. O encadeamento save/sync está em `app/ferias/models.py:434`. |
| Remarcar depois / corrigir agora | Ausência D → AG U → Ausência I → PA U. (`app/ferias/services.py:387`, `app/ferias/services.py:389`, `app/ferias/services.py:391`, `app/ponto/services/ausencia.py:1186`) |
| Corrigir antes | Mesma sequência, conforme corpo renomeado no diff fornecido; histórico independente: **[nao verificado]**. |
| Cancelar admin | PA U → Ausência U rejeitada → AG U → Ausência D. (`app/ferias/services.py:355`, `app/ferias/services.py:356`, `app/ferias/services.py:362`, `app/ponto/services/ausencia.py:1172`) |
| Gozo histórico | AG I → Ausência I → PA U. (`app/ferias/services.py:298`, `app/ferias/services.py:302`, `app/ferias/models.py:437`) |
| Executar | AG I → Ausência I → PA U. (`app/ferias/services.py:321`, `app/ferias/services.py:326`, `app/ferias/models.py:437`) |
| Avaliar/aprovar solicitação | Trava Solicitação → Solicitação U → AG I → Ausência I → PA U. (`app/ferias/views.py:717`, `app/ferias/views.py:718`, `app/ferias/services.py:416`, `app/ferias/services.py:441`, `app/ferias/services.py:444`) |
| Excluir período | DELETE do queryset de AG → DELETE do PA; esse corpo não chama explicitamente a limpeza individual. (`app/ferias/services.py:476`) |

Portanto, Ausência → AG é nova **nessa remarcação**, mas já ocorre no cancelamento admin e no corpo anterior da correção fornecido. Ausência → PA e PA → Ausência coexistem; não há ordem global uniforme. Deadlock concreto, linhas compartilhadas e ordem SQL dos efeitos indiretos: **[nao verificado]**. (`app/ferias/services.py:355`, `app/ferias/services.py:356`, `app/ferias/services.py:362`, `app/ferias/services.py:387`, `app/ferias/services.py:391`)

Os signals de Ausência acrescentam julgamento síncrono de células; os consumidores por `on_commit` ficam depois do commit. A sequência simplificada acima não é uma enumeração de todas as travas SQL. (`app/ponto/signals.py:85`, `app/ponto/signals.py:143`, `app/ponto/signals.py:150`, `app/ponto/signals.py:272`)

Os testes existentes mantêm expectativas de datas, saldo, recusas de status, permissões, justificativa, limpeza na correção e status rederivado. Não localizei expectativa que exija preservar as órfãs antigas; resultado real desses testes: **[nao verificado]**. (`app/ferias/test_editar_agendamento.py:25`, `app/ferias/test_editar_agendamento.py:35`, `app/ferias/test_editar_agendamento.py:52`, `app/ferias/test_editar_datas_admin.py:50`, `app/ferias/test_editar_datas_admin.py:96`, `app/ferias/test_a3_situacao_de_ferias.py:549`, `app/ferias/test_a3_situacao_de_ferias.py:571`)

**3. Contraexemplos e lacunas**

**Confirmação perdida — regressão permitida pelo save ampliado.** Intercalação: a remarcação carrega AG com `aguardando/None`; o colaborador confirma e grava `confirmado/instante`; a remarcação salva seu objeto antigo integralmente e pode restaurar `aguardando/None`. O teste novo não observa esses campos. É contraexemplo deduzido do código; reprodução em banco e ocorrência em produção: **[nao verificado]**. (`app/ferias/views.py:638`, `app/ferias/views.py:879`, `app/ferias/views.py:899`, `app/ferias/services.py:496`, `app/ferias/services.py:498`, `app/ferias/services.py:389`, `app/ferias/test_remarcar_limpa_janela_f346.py:52`)

**Duas remarcações deixam órfã — cura ainda vulne

[truncado em 20000 de 24159 caracteres]

## 05/11 22:08 · sessao c88468a062ea · - · - · origem=Ronald (tmux)

Censo estático da cópia, sem banco/log e sem execução de testes; cobertura de caminhos dinâmicos ou SQL externo: [nao verificado]. O lookup remete ao registro vivo (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`); saldo, status do PA, fracionamento, aviso e colisão têm autoridades registradas (`app/core/juizes.py:239`–`257`).

**Escritores de runtime**

| Campo | Escritores e persistência |
|---|---|
| AG `data_inicio/data_fim/dias` existente | `_trocar_janela`, atribuição e `save()` (`app/ferias/services.py:376`–`389`). Não encontrei outro escritor do intervalo por atribuição AST no código de runtime; inexistência de escritor dinâmico: [nao verificado]. |
| AG criação | `registrar_gozo_historico` (`app/ferias/services.py:278`–`298`), `executar_agendamento` (`app/ferias/services.py:321`–`323`), `aprovar_solicitacao` (`app/ferias/services.py:431`–`441`). |
| AG `aviso_emitido` | Criação comum (`app/ferias/services.py:314`, `323`); troca (`app/ferias/services.py:380`); aprovação (`app/ferias/services.py:436`). Default falso (`app/ferias/models.py:305`). |
| AG `status` | Criação histórica pelo juiz (`app/ferias/services.py:282`); default agendado (`app/ferias/models.py:309`); cancelamento DP (`app/ferias/services.py:336`–`337`), admin (`app/ferias/services.py:361`–`362`); troca pelo juiz (`app/ferias/services.py:384`); cron delega a `alinhar_status_por_data` (`app/ferias/management/commands/sincronizar_status_ferias.py:38`–`49`; `app/ferias/services.py:512`–`516`). |
| PA `dias_concedidos` | Histórico (`app/ferias/services.py:299`–`302`), criação (`app/ferias/services.py:324`–`326`), cancelamento DP (`app/ferias/services.py:333`–`335`), admin (`app/ferias/services.py:353`–`355`), troca (`app/ferias/services.py:378`, `391`), aprovação (`app/ferias/services.py:442`–`444`); defaults/criação via `garantir_periodo_aquisitivo` (`app/ferias/services.py:259`–`260`; `app/ferias/views.py:174`–`184`). |
| PA `status` | `aplicar_status` atribui a decisão de `status_de_periodo` (`app/ferias/models.py:204`–`212`); os escritores acima a persistem. Também sincronização (`app/ferias/services.py:225`–`239`), período existente por `QuerySet.update` (`app/ferias/services.py:263`–`266`), concessão com abono (`app/ferias/services.py:163`–`165`), venda (`app/ferias/services.py:506`–`508`). |

**Colunas adicionais alcançadas pelo `ag.save()` completo**

O modelo declara `periodo_aquisitivo`, `agendado_por`, `obs`, `confirmacao`, `confirmado_em`, `criado_em`, além dos cinco campos pretendidos (`app/ferias/models.py:299`–`319`). O PK é usado para identificar a linha; não deve ser contado como coluna regravada no UPDATE normal. Detalhe SQL da versão instalada: [nao verificado].

- `periodo_aquisitivo` e `agendado_por`: preenchidos nas três criações (`app/ferias/services.py:278`–`284`, `321`–`323`, `431`–`438`). São lidos na materialização, guardas e apresentação (`app/ponto/services/ausencia.py:1163`, `1189`; `app/ferias/models.py:424`, `428`; `app/templates/ferias/_tabela_ferias.html:36`–`38`).
- `obs`: criado nas mesmas portas (`app/ferias/services.py:284`, `323`, `438`); cancelamento admin concatena e persiste (`app/ferias/services.py:360`–`362`). `_trocar_janela` passa a regravar o snapshot sem atribuir novo valor (`app/ferias/services.py:379`–`389`).
- `confirmacao/confirmado_em`: `pedir_confirmacao` e `registrar_resposta_do_colab` (`app/ferias/services.py:481`–`498`). Leitores: envio (`app/ferias/views.py:838`), resposta (`app/ferias/views.py:879`), atendimento (`app/chamados/views.py:627`–`629`), apresentação (`app/templates/ferias/_secao_colab.html:122`–`124`).
- `criado_em`: `auto_now_add`, sem escritor explícito adicional localizado (`app/ferias/models.py:319`); leitor específico de runtime não localizado: [nao verificado].
- Exemplo de alteração concorrente exposta: depois de a view carregar AG, uma confirmação grava `confirmado/confirmado_em`; o `save()` completo da remarcação pode regravar o snapshot anterior. A view carrega antes do serviço (`app/ferias/views.py:638`, `668`), a confirmação grava separadamente (`app/ferias/services.py:496`–`498`) e a troca salva todos os campos (`app/ferias/services.py:389`). Ocorrência real: [nao verificado].

**Leitores de AG, agrupados**

- Núcleo: propriedades de estado (`app/ferias/models.py:322`–`346`); `clean` lê status/dias/datas (`app/ferias/models.py:350`–`375`); guarda lê datas/status e consulta janela persistida (`app/ferias/models.py:394`–`430`); texto e ordenação (`app/ferias/models.py:448`–`451`); saldo para remarcar lê `dias` (`app/ferias/models.py:219`–`222`).
- Serviços: intervalo e status (`app/ferias/services.py:51`, `63`, `77`–`82`, `102`); duplicata (`app/ferias/services.py:316`–`318`); cancelamentos e troca (`app/ferias/services.py:333`–`389`).
- Views: KPIs/listagem (`app/ferias/views.py:25`–`31`, `71`–`89`); validação/fracionamento (`app/ferias/views.py:234`); cancelamento/auditoria (`app/ferias/views.py:325`, `362`–`375`, `404`–`414`); correção/auditoria (`app/ferias/views.py:452`, `470`–`503`); lote (`app/ferias/views.py:527`, `561`); remarcação (`app/ferias/views.py:649`, `671`); exclusão de PA (`app/ferias/views.py:814`); confirmação/push (`app/ferias/views.py:838`, `849`, `856`, `879`, `891`).
- Situação: frações (`app/ferias/situacao.py:117`–`122`), vivos (`app/ferias/situacao.py:164`–`168`), aviso (`app/ferias/situacao.py:246`–`262`), batida dentro das férias (`app/ferias/situacao.py:270`–`289`), aviso de colisão (`app/ferias/situacao.py:343`–`352`).
- Espelho Ausência: `status/data_inicio/data_fim/agendado_por` (`app/ponto/services/ausencia.py:1163`–`1190`); pauta de recusa lê datas (`app/ponto/services/ausencia.py:1235`–`1236`).
- Folha: lê intervalo pelo serviço e recorta (`app/folha/export.py:473`–`482`).
- API: datas/dias/status (`app/api/views.py:2252`–`2268`); mensageria também aviso (`app/api/views_mensageria.py:840`–`843`).
- Disponibilidade: consulta status/fim e retorno (`app/colaboradores/views.py:2843`–`2853`), em gozo pelo juiz e apresentação (`app/colaboradores/views.py:2895`–`2904`).
- Inteligência: consulta início/status e apresentação início/dias (`app/inteligencia/views.py:92`–`94`, `276`–`289`).
- Ponto: gate/apresentação (`app/ponto/utils.py:88`–`131`); universo por janela (`app/ponto/utils.py:197`–`198`); aviso da batida pelo juiz (`app/ponto/services/aviso_da_batida.py:73`–`74`).
- Comandos: sincronização (`app/ferias/management/commands/sincronizar_status_ferias.py:33`–`43`); retornos (`app/ferias/management/commands/alertar_retornos.py:20`–`24`).
- Templates: datas/dias/status/aviso (`app/templates/ferias/_tabela_ferias.html:39`–`43`, `app/templates/ferias/saldo.html:59`–`70`, `app/templates/ferias/_secao_colab.html:117`–`152`); lote (`app/templates/ferias/cancelar_admin_massa.html:42`–`45`); atendimento (`app/templates/ferias/_meu_atendimento_ferias.html:6`).

**Leitores de PA `dias_concedidos/status`**

Saldo/status canônicos (`app/ferias/models.py:95`, `212`, `217`); sincronização e divergência (`app/ferias/services.py:90`, `225`–`239`), períodos vencendo (`app/ferias/services.py:118`), todos os incrementos/decrementos acima; seleção da aprovação (`app/ferias/services.py:421`–`423`); listagem e KPIs (`app/ferias/views.py:29`–`30`, `55`–`59`); auditoria (`app/ferias/views.py:364`, `373`, `406`, `414`, `473`, `503`); proteção de direito (`app/ferias/views.py:790`–`791`); situação de férias (`app/ferias/situacao.py:178`–`183`); relatório (`app/relatorios/services.py:285`–`290`); templates (`app/templates/ferias/saldo.html:20`–`46`, `app/templates/ferias/_secao_colab.html:42`–`49`, `app/templates/ferias/_tabela_ferias.html:29`).

**Ausencia `ferias`: escritores**

A materialização lê todas as linhas do tipo na janela, sem filtro de status (`app/ponto/services/ausencia.py:1165`–`1169`); cria os dias faltantes pela porta genérica (`app/ponto/services/ausencia.py:1182`–`1191`) e apaga quando inativa (`app/ponto/services/ausencia.py:1172`–`1175`). Limpeza inclusiva com exceção inclusiva (`app/ponto/services/ausencia.py:1250`–`1254`); rejeição admin (`app/ponto/services/ausencia.py:1265`–`1267`); AG `delete()` chama limpeza (`app/ferias/models.py:439`–`443`).

Portas genéricas existentes também alcançam registros de férias: criação (`app/ponto/services/ausencia.py:130`, `341`–`349`, `417`), edição (`app/ponto/services/ausencia.py:465`, `496`–`508`), estado (`app/ponto/services/ausencia.py:792`, `808`–`819`), decisão (`app/ponto/services/ausencia.py:895`–`911`), reabertura (`app/ponto/services/ausencia.py:1090`–`1132`), rederivação (`app/ponto/services/ausencia.py:1277`–`1280`), limpeza exclusiva de ensaio (`app/ponto/services/ausencia.py:1270`–`1274`). Todas ficam na mesma porta; não constituem automaticamente escritor externo. Admin de Ausencia é `SoLeitura` (`app/ponto/admin.py:37`–`43`); admin de férias registra apenas Afastamento (`app/ferias/admin.py:2`–`5`).

**Ausencia: leitores e efeitos relevantes**

- Cobertura e universos: `app/ponto/turnos.py:2529`, `2552`, `2753`, `2815`, `3087`; precedência/veto: `app/ponto/precedencia.py:273`–`279`; cartório: `app/ponto/services/cartorio.py:355`; decisão do dia: `app/ponto/services/dia_decidido.py:481`–`487`, `542`, `579`.
- DELETE/INSERT dispara leitura dos dias e rejulgamento síncrono (`app/ponto/signals.py:115`–`128`, `143`–`152`) e reconciliação após commit (`app/ponto/signals.py:250`–`284`).
- Consumidores genéricos: ponto/telas (`app/ponto/views.py:1002`, `2182`, `2222`, `2335`, `2573`, `2718`, `2751`); API (`app/api/views.py:1861`, `3092`); detalhe do colaborador (`app/colaboradores/services/detalhe.py:104`); histórico (`app/chamados/services/historico.py:129`); texto/absorção (`app/chamados/services/materializacao.py:599`–`600`, `635`–`653`); folha (`app/folha/export.py:76`–`81`); relatórios (`app/relatorios/views.py:197`, `277`); inteligência (`app/inteligencia/resumo.py:39`, `app/inteligencia/views.py:71`, `239`, `516`); vigias (`app/ponto/services/vigia_ausencia.py:59`, `108`–`120`, `215`–`219`; `app/ponto/services/vigia_ausencia_veredito.py:46`).
- Catálogo/contagem lê qualquer tipo (`app/ponto/views_tipos_ausencia.py:24`). Comandos genéricos de inspeção/rejulgamento: `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`, `ausencias_sem_efeito_na_celula.py:41`, `ausencia_bloqueante_sem_fim.py:27`, `curar_ausencia_sem_fim.py:29`, `rejulgar_status_por_tipo.py:23`, `reabrir_chamados_ausencia_aberta.py:33`.

**Fontes distintas de runtime**

Schema/defaults iniciais (`app/ferias/migrations/0001_initial.py:40`–`72`), confirmação (`app/ferias/migrations/0003_agendamentoferias_confirmacao_and_more.py:13`–`21`). Fixtures de teste criam AG/PA diretamente, p.ex. `app/ferias/tests.py:96`, `app/ferias/test_editar_agendamento.py:16`–`22`, `app/ferias/test_editar_datas_admin.py:31`–`42`, teste novo `app/ferias/test_remarcar_limpa_janela_f346.py:29`–`32`. Há injeção intencional de legado ultrapassando o override (`app/ferias/test_sobreposicao_chokepoint.py:60`–`63`). Não se deve converter isso em escritor de produção.

Outro sítio de troca do intervalo AG sem limpeza não foi encontrado na busca estática; afirmação absoluta sobre caminhos dinâmicos, importação externa ou dado de produção: [nao verificado].

## 05/11 22:08 · sessao 5342cbf7a899 · - · - · origem=Ronald (tmux)

A cura possui mordida funcional e estrutural, com lacunas de cobertura; não encontrei contraexemplo atual à diferença inclusiva entre janelas. (app/ferias/test_remarcar_limpa_janela_f346.py:52; app/ponto/services/ausencia.py:1246)

- O teste funcional percorre a porta HTTP real e verifica janela disjunta, sobreposição com preservação dos PKs, borda comum inclusiva, redução e repetição; R01 também confere as datas persistidas do agendamento. A aceitação HTTP 200/302 isolada é fraca, mas as asserções posteriores impedem que uma simples recusa da remarcação faça esses cenários passarem. (app/ferias/test_remarcar_limpa_janela_f346.py:43; app/ferias/test_remarcar_limpa_janela_f346.py:52)
- Pela leitura do diff fornecido, a implementação anterior manteria as ausências antigas em R01/R02/R03/R04; execução RED contra base/pai não foi feita nem evidenciada nesta auditoria. [nao verificado]
- O detector AST lê somente `services.py`, reconhece apenas `Assign`/`AugAssign` para atributos chamados `data_inicio`, `data_fim` ou `dias`, e exclui receptores literalmente chamados `periodo` e `self`. A contraprova embutida morde dois escritores com atribuição explícita; não certifica o censo geral. (app/ferias/test_remarcar_limpa_janela_f346.py:87; app/ferias/test_remarcar_limpa_janela_f346.py:91; app/ferias/test_remarcar_limpa_janela_f346.py:130)
- Extraí e executei apenas os helpers AST em memória, com Python3 `-B`: a cópia retorna `['_trocar_janela']`; acrescentar segundo escritor via `setattr`, `QuerySet.update` ou `AnnAssign` mantém esse resultado, enquanto atribuição explícita acrescenta `b`. Esses escapes decorrem das formas selecionadas pelo detector. (app/ferias/test_remarcar_limpa_janela_f346.py:99)
- Mutante plausível: retirar o `atomic` e manter a mesma limpeza e demais instruções passa pelos cenários novos de sucesso e pelo AST; faltaria teste que provoque A2 depois do DELETE e confira restauração das ausências antigas e do PA. A implementação atual protege essas operações com o mesmo `atomic`; não é defeito atual. (app/ferias/services.py:377; app/ferias/services.py:387; app/ferias/test_remarcar_limpa_janela_f346.py:52; app/ferias/test_remarcar_limpa_janela_f346.py:120)

Os únicos chamadores `.py` encontrados das duas funções são `editar_datas_admin` e `editar_agendamento`; a correção captura `ValidationError`, enquanto a remarcação a deixa propagar. O veto continua acontecendo no `save`, logo essa diferença entre as portas já existia antes da cura. (app/ferias/views.py:489; app/ferias/views.py:491; app/ferias/views.py:667; app/ferias/models.py:427; app/ferias/models.py:434)

O `save(update_fields=[datas…])` anterior já disparava `_guard_sobreposicao` e `_sync_ausencia`; `save()` completo amplia as colunas persistidas, não cria esses disparos. Além dos cinco campos tratados, passa a persistir `periodo_aquisitivo_id`, `agendado_por_id`, `obs`, `confirmacao`, `confirmado_em` e `criado_em`; a confirmação possui escritor concorrente possível em `registrar_resposta_do_colab`, portanto uma instância carregada antes dessa resposta pode sobrescrevê-la com valores antigos. Ocorrência efetiva em produção: [nao verificado]. (app/ferias/models.py:299; app/ferias/models.py:310; app/ferias/models.py:313; app/ferias/models.py:434; app/ferias/services.py:389; app/ferias/services.py:489)

Testes existentes relevantes preservam suas expectativas: [nao verificado]

- Remarcação: datas novas, recusa de `em_gozo`/`concluido`, saldo e dias zero. A mudança das ausências não era observada nesses testes. (app/ferias/test_editar_agendamento.py:25; app/ferias/test_editar_agendamento.py:35; app/ferias/test_editar_agendamento.py:52; app/ferias/test_editar_agendamento.py:62)
- Correção admin: permissão, justificativa, cancelado, edição de `em_gozo`/`concluido`, saldo, migração das ausências e manutenção da porta antiga estreita. (app/ferias/test_editar_datas_admin.py:50; app/ferias/test_editar_datas_admin.py:71; app/ferias/test_editar_datas_admin.py:88; app/ferias/test_editar_datas_admin.py:96; app/ferias/test_editar_datas_admin.py:108)
- Correção A3: fração curta permitida, colisão recusada, saldo recusado e status rederivado no mesmo ato. O teste de colisão verifica somente a data antiga do agendamento, sem certificar as ausências restauradas. (app/ferias/test_a3_situacao_de_ferias.py:538; app/ferias/test_a3_situacao_de_ferias.py:549; app/ferias/test_a3_situacao_de_ferias.py:561; app/ferias/test_a3_situacao_de_ferias.py:571)

Sobre os casos pedidos: borda e sobreposição à direita estão cobertas; à esquerda, expansão, contenção deslocada, A2 e duas janelas vizinhas não estão no teste novo. O helper atual opera `antiga − nova` por limites inclusivos e, para vizinhas sem sobreposição, não alcança os dias do outro agendamento. A2 restaura o DELETE pelo `atomic`, embora a porta HTTP de remarcação não trate sua exceção. Um status diferente de `agendado` chegando diretamente ao serviço é rederivado sem guarda adicional; a guarda está explicitamente na view, e acrescentá-la ao serviço seria a guarda copiada proibida pelo aval. (app/ferias/test_remarcar_limpa_janela_f346.py:52; app/ponto/services/ausencia.py:1250; app/ferias/services.py:377; app/ferias/services.py:384; app/ferias/services.py:400; app/ferias/views.py:649; app/ferias/views.py:667)

## 05/11 22:07 · sessao 3acab27597a4 · - · - · origem=Ronald (tmux)

Auditoria de ordem/autoridades concluída por leitura; ocorrência real de deadlock ou resultados de execução permanecem [nao verificado].

**Ordens das três tabelas do domínio**

| Caminho | Ordem de escrita comprovável |
|---|---|
| Remarcar antes | AG UPDATE → Ausencia INSERT pelo sync → PA UPDATE; o corpo anterior está no diff fornecido, e o encadeamento de `save` para sync está em `app/ferias/models.py:434-437`. Referência de linha para o corpo anterior: [nao verificado]. |
| Remarcar depois / corrigir agora | Ausencia DELETE → AG UPDATE → Ausencia INSERT pelo sync → PA UPDATE (`app/ferias/services.py:377-391`, `app/ferias/models.py:434-437`, `app/ponto/services/ausencia.py:1182-1194`). |
| Corrigir antes | Mesma ordem atual, pois o diff fornecido apenas renomeia seu corpo para `_trocar_janela`; referência de linha do arquivo anterior: [nao verificado]. |
| Cancelar admin | PA UPDATE → Ausencia UPDATE rejeitada → AG UPDATE → Ausencia DELETE pelo sync (`app/ferias/services.py:352-362`, `app/ponto/services/ausencia.py:1265-1267`, `app/ponto/services/ausencia.py:1172-1176`). |
| Gozo histórico | AG INSERT → Ausencia INSERT → PA UPDATE (`app/ferias/services.py:277-302`, `app/ferias/models.py:434-437`). |
| Executar agendamento | AG INSERT → Ausencia INSERT → PA UPDATE (`app/ferias/services.py:315-326`, `app/ferias/models.py:434-437`). |
| Avaliar solicitação / aprovar | SELECT FOR UPDATE Solicitação → Solicitação UPDATE → AG INSERT → Ausencia INSERT → PA UPDATE (`app/ferias/views.py:717-731`, `app/ferias/services.py:413-416`, `app/ferias/services.py:431-444`). |
| Excluir período | DELETE em queryset dos agendamentos → DELETE do período; não há chamada explícita à porta de limpeza nesse corpo (`app/ferias/services.py:476-478`). |

- A ordem **A→AG é nova na remarcação**, mas não é par novo no conjunto analisado: já era a ordem da correção e cancelamento admin também acessa A antes de AG (`app/ferias/services.py:356-362`, `app/ferias/services.py:387-389`; histórico da correção pelo diff: [nao verificado]).
- Existem ordens opostas **A→PA** e **PA→A** entre troca/criação e cancelamento admin; portanto não cabe afirmar ordem global consistente. O diff amplia para remarcação uma ordem já existente na correção, sem demonstrar introdução de um novo par global (`app/ferias/services.py:387-391`, `app/ferias/services.py:353-356`, `app/ferias/services.py:321-326`).
- Deadlock concreto exige linhas concorrentes realmente compartilhadas e execução sobre banco; ocorrência, frequência e planos de trava são [nao verificado]. Não confundir a atribuição Python `periodo.dias_concedidos = ...` com UPDATE: a escrita do PA só ocorre no `save` posterior (`app/ferias/services.py:378`, `app/ferias/services.py:391`).

**Efeitos indiretos que a sequência simplificada omite**

- Ausencia tem consumidores síncronos de `post_save` e `post_delete` que chamam julgamento de células; portanto DELETE/INSERT não são operações isoladas das demais tabelas (`app/ponto/signals.py:143-152`, `app/ponto/signals.py:85-93`).
- CelulaDia referencia Ausencia por `SET_NULL`, logo apagar uma ausência também afeta esse vínculo quando existente (`app/escala/models.py:1796-1798`). A ordem SQL exata do coletor Django não foi disponível na cópia: [nao verificado].
- O consumidor de chamados agenda execução por `transaction.on_commit`, portanto esse consumidor não deve ser colocado entre DELETE e `ag.save()` na ordem interna do atomic (`app/ponto/signals.py:250-272`, `app/ponto/signals.py:282-284`).
- O julgamento síncrono retorna para datas futuras; os cenários novos, todos futuros ao relógio congelado, não exercitam lavra de células passadas (`app/ponto/services/cartorio.py:1965-1978`, `app/ferias/test_remarcar_limpa_janela_f346.py:19-30`).
- Em veto A2, a exceção sobe de `AgendamentoFerias.save` antes do `super().save`; o DELETE anterior está dentro do mesmo atomic. O código promete rollback transacional, mas execução do rollback nesta cópia é [nao verificado] (`app/ferias/models.py:428-436`, `app/ferias/services.py:377-389`).

**Juiz, derivação e relógio**

- JUIZES é lookup; o registro vivo prevalece (`app/docs/JUIZES.md:10-14`). Autoridades relevantes: veto, saldo, status de PA, fracionamento e aviso (`app/core/juizes.py:242`, `app/core/juizes.py:252-256`).
- Não identifiquei novo juiz nesses corpos: `_trocar_janela` chama `status_por_data`, `periodo.aplicar_status` e a porta de ausência; o save chama o veto registrado (`app/ferias/services.py:373-390`, `app/ferias/models.py:212`, `app/ferias/models.py:428-432`).
- Não há nova derivação paralela nesses corpos: o status de AG vem de `status_por_data`, o de PA de `status_de_periodo`, o aviso usa a constante registrada (`app/ferias/services.py:380-390`, `app/ferias/models.py:212`, `app/core/juizes.py:256`).
- Há relógio lido dentro do serviço, em vez de recebido da borda, além dos defaults dos juízes: `date.today()` no aviso e nos juízes (`app/ferias/services.py:380`, `app/ferias/models.py:280`, `app/ferias/models.py:94`). O diff preserva leituras já existentes em ambas as funções anteriores; não inaugura esse padrão [nao verificado quanto às linhas do commit anterior].
- Chamadas distintas ao relógio podem teoricamente atravessar meia-noite; a execução dessa divergência e sua presença em produção são [nao verificado]. O teste congela o relógio, não verifica propagação de um único instante capturado pela borda (`app/ferias/test_remarcar_limpa_janela_f346.py:23`).

**Grep pedido**

Nos termos literais `ferias/services`, `apagar_ausencias_de_ferias`, `remarcar`, `_sync_ausencia`, `espelhar_ferias`, a única ocorrência encontrada em LEIS/CORTES/DOSSIES foi `app/docs/LEIS.md:174`, L-089, citando `aplicar_art1`. Ausência de outra regra sob vocabulário diferente: [nao verificado].

**Outros pontos úteis**

- A porta admin captura `ValidationError`; a porta remarcação chama o serviço sem esse tratamento, podendo propagar o veto como erro HTTP. Essa diferença já existia antes segundo o diff, não nasce da delegação (`app/ferias/views.py:488-494`, `app/ferias/views.py:667-668`).
- A guarda HTTP da remarcação só testa `status == 'agendado'`; não consulta `ja_iniciou` nem compara início com hoje. Assim um agendamento atrasado cujo cron ainda não atualizou o status passa pela guarda (`app/ferias/views.py:648-650`, `app/ferias/models.py:322-328`).
- O serviço compartilhado não guarda status: chama o juiz temporal e pode reativar um cancelado se chamado diretamente; as duas portas HTTP possuem guardas diferentes que o impedem nos caminhos usuais (`app/ferias/services.py:378-389`, `app/ferias/views.py:452-453`, `app/ferias/views.py:648-650`). Isso também existia nos dois corpos anteriores segundo o diff; ocorrência real é [nao verificado].

## 05/11 22:05 · sessao 096a16709250 · C20-FIO-MUDO · verificador · origem=Code

**Parecer: não certificaria a obra como completa.** A escrita continua pelas portas existentes, mas há eleição paralela da canônica, retratação antes da aceitação da costura e falta de garantia transacional na reabertura direta. (`app/chamados/models.py:484`, `app/chamados/models.py:487`, `app/chamados/models.py:492`, `app/chamados/models.py:1408`, `app/chamados/models.py:1478`)

Execução dos testes, reprodução das corridas e resultado real em produção: **[não verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

| Campo/ato | Escritores | Leitores relevantes |
|---|---|---|
| `PerguntaDisputa.chamado_destino` | `PerguntaDisputa.abrir`, tanto nascimento quanto reapontamento por identidade; `fio.reapontar_resolvedora`. O método novo chama essa porta. (`app/chamados/models.py:2526`, `app/chamados/models.py:2552`, `app/chamados/services/fio.py:845`, `app/chamados/models.py:492`) | Âncora, C6/materialização, plano e reconciliador. (`app/chamados/services/fio.py:117`, `app/chamados/juizes.py:817`, `app/chamados/juizes.py:744`, `app/chamados/models.py:344`, `app/chamados/reconciliador.py:1480`) |
| `via_resolucao` | Núcleo `_escrever_carimbo`; portas de carimbar/descarimbar; também os sítios existentes de relavra e encerramento por fato. O novo caminho passa por `retratar_duplicata → carimbar_identidade`. (`app/chamados/models.py:2632`, `app/chamados/models.py:2701`, `app/chamados/models.py:2732`, `app/chamados/models.py:2749`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/fio.py:891`) | C6; migrador, por C6; ressuscitação. (`app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`, `app/chamados/services/fio.py:935`, `app/chamados/models.py:1531`) |
| `validada_em` | Validação/devolução administrativa; simetria em `carimbar`; desfazimento por correção e ressuscitação. Não recebe escritor novo neste diff. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/models.py:2668`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1580`) | C6, estado, seleção de ancoradas e reconciliação. (`app/chamados/juizes.py:1136`, `app/chamados/models.py:2575`, `app/chamados/models.py:327`, `app/chamados/reconciliador.py:620`) |
| `status_local` | `_gravar_estado` e nascimento pelas portas existentes. O helper novo somente lê. (`app/chamados/models.py:222`, `app/chamados/models.py:1006`, `app/chamados/models.py:452`, `app/chamados/models.py:469`) | Vigia, gates, guarda da costura e juiz de materialização. (`app/chamados/services/vigia_fio.py:335`, `app/chamados/models.py:400`, `app/chamados/services/fio.py:764`, `app/chamados/juizes.py:744`) |
| Evento `pergunta_reatada` | Gate anterior e helper novo chamam `core.observ.evento`. Antes, `objeto_id` identifica a âncora que fecha; agora também pode identificar o destino que abre. (`app/chamados/models.py:423`, `app/chamados/models.py:496`) | Teste específico e auditoria genérica. Consumidor de produção específico por esse nome: **[não verificado]**. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:302`, `app/core/views.py:705`) |
| Lavra do reapontamento | `veredito_lavrado.lavrar` calcula os três campos; `reapontar_resolvedora` grava destino e lavra no mesmo `save`. (`app/chamados/veredito_lavrado.py:211`, `app/chamados/veredito_lavrado.py:227`, `app/chamados/services/fio.py:845`) | Leitura do veredito e controle de envelhecimento do cache. (`app/chamados/juizes.py:632`, `app/chamados/veredito_lavrado.py:173`) |

**Não há segundo escritor direto novo fora da porta.** A autorização do destino já contempla `models.py` e `fio.py`; o diff acrescenta uma chamada, não uma atribuição paralela. (`app/core/tests/test_contract_escritores_estado.py:60`, `app/chamados/models.py:492`)

Há, porém, resultados diferentes entre leitores:

- `fio_vivo` reduz C6; o vigia delega à mesma redução. Reatar muda a âncora, sem criar outra definição de vivacidade. (`app/chamados/juizes.py:1233`, `app/chamados/services/vigia_fio.py:156`)
- O helper aceita pergunta de disputa fechada e não reabre seu continente. O admin deliberadamente mostra essas vivas, mas `_disputa_do_chamado` exige disputa aberta e o caminho do colaborador também preserva esse recorte. Portanto, “reatada e viva” não prova “questionário novamente acessível ao colaborador”. (`app/chamados/models.py:464`, `app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:209`, `app/api/views.py:1008`)
- A ressuscitação existente reabre a disputa quando efetivamente devolve perguntas; a adoção não executa esse efeito. Isso é uma diferença de cobertura, sem autorização para simplesmente reabrir tudo. (`app/chamados/models.py:1580`, `app/chamados/models.py:1607`, `app/chamados/services/fio.py:211`)

**2. Juiz novo ou derivação paralela**

Dia, motivos, competência, vivacidade e âncora são consultados nas funções existentes; não identifiquei cópia nova dessas cinco regras. O registro mantém C6 e a redução do fio nas autoridades já declaradas. (`app/chamados/models.py:454`, `app/chamados/models.py:459`, `app/chamados/models.py:468`, `app/chamados/models.py:475`, `app/core/juizes.py:260`)

A diferença para `_plano_pergunta_viva` é o sentido da busca: o plano parte da âncora e procura uma cobrança sobrevivente, conferida por E4-c; o novo método parte da cobrança e procura perguntas do colaborador/dia/motivo. (`app/chamados/models.py:364`, `app/chamados/models.py:373`, `app/chamados/models.py:464`)

**Existe uma segunda regra de eleição:** `_ja` determina que “a que já está aqui vence”, sem consultar `canonica_do_grupo` sobre o grupo completo. Exemplo: pergunta local sem resposta e órfã respondida; a lei da canônica favorece a respondida, mas o helper ignora a órfã inteira. A migração existente compara origem e destino conjuntamente. (`app/chamados/models.py:477`, `app/chamados/models.py:484`, `app/chamados/services/fio.py:873`, `app/chamados/services/fio.py:937`)

**3. Ordem de trava**

- `abrir()` envolve a seleção e criação em `atomic`, adquirindo primeiro a trava do colaborador. (`app/chamados/models.py:937`, `app/chamados/models.py:940`)
- **`reabrir()` diretamente não faz o mesmo:** consulta estado sem trava, chama `_gravar_estado`, depois ressuscita e adota. O `atomic` interno de `_gravar_estado` não envolve toda essa sequência. Quando chamado por `abrir`, herda a transação externa; fora disso, não há a mesma garantia. (`app/chamados/models.py:1408`, `app/chamados/models.py:274`, `app/chamados/models.py:1468`, `app/chamados/models.py:1477`)
- `canonica_do_grupo` somente ordena objetos em memória; competência consulta janela e entrega, sem trava explícita nesses helpers. (`app/chamados/services/fio.py:873`, `app/chamados/services/vigia_fio.py:834`)
- `retratar_duplicata` grava a pergunta pela porta de identidade; `reapontar_resolvedora` abre `atomic`, trava o chamado de destino e depois grava a pergunta canônica. Não trava nem relê a pergunta antes da decisão. (`app/chamados/services/fio.py:891`, `app/chamados/models.py:2720`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:762`, `app/chamados/services/fio.py:847`)

Não identifiquei inversão **direta nova** de Disputa→Colaborador nesses helpers: eles não adquirem `select_for_update` de Disputa. O callback da pergunta agenda becos em `on_commit`. Isso não constitui prova geral contra deadlock; o próprio selo declara que não analisa chamadas entre funções. (`app/chamados/services/fio.py:742`, `app/chamados/services/fio.py:878`, `app/chamados/signals.py:191`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:14`)

**4. Relógio e orçamento de queries**

O método novo não lê relógio diretamente nem recebe instante. Entretanto, chama C6, que pode chegar ao `date.today()` decisório preexistente de `_dentro_da_janela_viva`. Portanto, não introduz essa leitura, mas amplia seu alcance dentro de `abrir` sem injetar relógio da borda. (`app/chamados/models.py:434`, `app/chamados/models.py:475`, `app/chamados/juizes.py:1197`, `app/chamados/juizes.py:562`)

Os `now()` de retratação e lavra são carimbos de escrita preexistentes; devem ser distinguidos daquele relógio decisório. A composição da competência recebe o dia explícito. (`app/chamados/services/fio.py:884`, `app/chamados/veredito_lavrado.py:228`, `app/chamados/services/vigia_fio.py:838`)

**Há custo por candidata:** a busca carrega somente `disputa`, enquanto C6 acessa célula, colaborador, empresa e tranca. O helper não aplica o mapa de identidade usado pela migração; `_st` resolve em lote apenas os estados das âncoras. (`app/chamados/models.py:464`, `app/chamados/models.py:469`, `app/chamados/juizes.py:801`, `app/chamados/juizes.py:932`, `app/chamados/services/fio.py:927`)

Os selos citados não certificam esse orçamento:

- `test_c2_corrida_do_abrir` usa módulo `sem_previsao_escala`, que sai na primeira guarda C20; mede deduplicação concorrente, não queries da adoção. (`app/chamados/tests/test_c2_corrida_do_abrir.py:56`, `app/chamados/tests/test_c2_corrida_do_abrir.py:127`, `app/chamados/models.py:452`)
- O chokepoint mede batida/reconciliação/estado; seu `abrir` de preparação está fora da captura correspondente. Quebra efetiva de teto: **[não verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:110`, `app/ponto/tests/test_selo_chokepoint_escrita.py:312`)

**5. Idempotência**

No sucesso sequencial normal, a segunda adoção encontra a pergunta em `self`, inclui seu motivo em `_ja` e não reaponta nem repete `pergunta_reatada`. (`app/chamados/models.py:477`, `app/chamados/models.py:484`)

Isso **não garante zero duplicata**:

- Se já houver uma viva local, a órfã equivalente permanece intocada. (`app/chamados/models.py:484`)
- Uma duplicata respondida pode continuar viva: C6 examina a resposta antes da via de identidade, e a porta documenta explicitamente essa exceção. A frase nova “duplicata já é morta pelo C6” é falsa como afirmação geral. (`app/chamados/models.py:446`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`, `app/chamados/models.py:2702`)

Dois `abrir` da mesma chave serializam pelo colaborador; o segundo retorna o existente sem nova adoção. Dois `reabrir` diretos não têm essa proteção integral. Ambas as execuções podem selecionar a mesma órfã antes da costura; a porta trava o destino, mas salva a pergunta recebida sem refazer a seleção. Duplicação de evento em corrida é um contraexemplo estático possível; reprodução: **[não verificado]**. (`app/chamados/models.py:940`, `app/chamados/models.py:959`, `app/chamados/models.py:1408`, `app/chamados/models.py:464`, `app/chamados/services/fio.py:845`)

“Uma linha” também depende do sucesso da auditoria: `evento` é best-effort e pode retornar sem gravá-la. (`app/core/observ.py:98`, `app/core/observ.py:105`, `app/core/observ.py:139`)

**6. Contraexemplos e fronteiras**

| Caso | Resultado demonstrável |
|---|---|
| Outro dia ou motivo | Fica fora do filtro. A busca também não alcança D±1 que E4-c admite como mesma jornada overnight; conformidade desse alcance com o aval: **[não verificado]**. (`app/chamados/models.py:464`, `app/chamados/juizes.py:1991`) |
| Outro módulo | Outro módulo de **destino novo** sai pela constante; módulo diferente da **âncora antiga terminal** não impede adoção. (`app/chamados/models.py:432`, `app/chamados/models.py:452`, `app/chamados/models.py:473`) |
| Morta por C6 ou validada | Não é adotada. A exceção de duplicata respondida segue a precedência do próprio C6. (`app/chamados/models.py:475`, `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1150`) |
| Competência entregue | Sai antes da busca. O renascimento admite competência exportada, portanto pode reabrir sem recuperar a órfã por esta cura. A exclusão copia a cerca existente; sua conciliação com L-113 permanece **[não verificado]**. (`app/chamados/models.py:460`, `app/chamados/models.py:347`, `app/chamados/models.py:1030`, `app/docs/LEIS.md:150`) |
| Agrupado/limbo | As entradas normais protegem agrupado sob pai vivo; o helper isolado não consulta agrupamento. Chamado de outro módulo, como destino de limbo, sai pela guarda inicial. (`app/chamados/models.py:960`, `app/chamados/models.py:1442`, `app/chamados/models.py:452`) |
| Pai de adesão/nunca-bateu | O pai de adesão usa outro módulo e não adota. Entretanto, uma órfã preexistente de um `batida_ausente` nunca-bateu pode passar: `nunca_bateu` acusa no C6. R19d não cobre esse estoque. (`app/chamados/services/adesao.py:74`, `app/escala/models.py:1767`, `app/chamados/juizes.py:1181`, `app/chamados/models.py:464`) |
| Destino terminal ou nenhum | Coberto quando C6 permite. “Sem destino” com pai vivo não é órfã: a âncora efetiva é o pai, preservado pela guarda. (`app/chamados/services/fio.py:117`, `app/chamados/models.py:473`, `app/chamados/models.py:492`) |
| Renascer por premissa | Alcança a adoção se efetivamente passar por `reabrir`; existente já vivo retorna sem reparar órfãs. (`app/chamados/models.py:1055`, `app/chamados/models.py:1478`, `app/chamados/models.py:959`) |

**Pouso antes do pré-requisito:** o método retrata irmãs antes de saber se a canônica será aceita. Exemplo estático: célula em D e contexto em D+2; a seleção usa D pela célula, E4-c compara o contexto e recusa, mas as retratações anteriores permanecem. A abertura continua. Execução desse cenário: **[não verificado]**. (`app/chamados/juizes.py:90`, `app/chamados/juizes.py:1973`, `app/chamados/juizes.py:1998`, `app/chamados/models.py:487`, `app/chamados/models.py:493`)

**7. Os cinco atos de perda**

| Ato | Cobertura |
|---|---|
| **Reabrir** | Novo gancho cobre reabertura efetiva de Chamado, após ressuscitar as próprias. `Disputa.reabrir_admin/sistema/por_colab` não chama diretamente a adoção; a casca administrativa chama `ch.reabrir` somente se encerrado. (`app/chamados/models.py:1477`, `app/chamados/models.py:2116`, `app/chamados/models.py:2153`, `app/chamados/models.py:2222`, `app/chamados/services/acoes_disputa.py:265`) |
| **Retratar duplicata** | A porta carimba a duplicata, mas não reata a canônica. Pode retirar a última viva de um chamado enquanto a canônica permanece num terminal; o diff não acrescenta evento nessa operação. (`app/chamados/services/fio.py:878`, `app/chamados/models.py:1009`, `app/chamados/models.py:1478`) |
| **Validação** | Já possui transação para carimbo/materialização/desfecho e recalcula por evento. A morte por validação é legítima; não exige irmã nova. O diff não acrescenta gatilho C20 aqui. (`app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:405`, `app/chamados/juizes.py:1136`) |
| **Morte por leitura** | C6 já resolve tranca, marco e célula; E2 carimba absolvidas pelo julgamento existente. Não há nova adoção nessa leitura. Morte legítima não demonstra perda indevida de cobertura. (`app/chamados/juizes.py:1138`, `app/chamados/juizes.py:1177`, `app/chamados/reconciliador.py:651`, `app/ponto/services/cartorio.py:958`) |
| **Reapontar** | A porta existente protege o **destino**, valida e lavra no mesmo save. Não verifica o chamado de origem que perde sua última pergunta. A legitimidade de deixar essa origem sem fio precisa ser demonstrada por caso. (`app/chamados/services/fio.py:784`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:845`) |

Assim, os dois ganchos novos não demonstram, sozinhos, cobertura dos cinco atos. (`app/chamados/models.py:1009`, `app/chamados/models.py:1478`)

**8. Mordida dos testes e R19d** [nao verificado]

R08/R08b exigem cobrança, destino correto, fio vivo e nenhuma irmã; R08r e R19b incluem guardas explícitas de vivacidade. São as verificações mais fortes do arquivo, mas execução RED e eficácia por mutação permanecem **[não verificado]**. (`app/chamados/tests/test_c20_fio_mudo.py:125`, `app/chamados/tests/test_c20_fio_mudo.py:164`, `app/chamados/tests/test_c20_fio_mudo.py:204`)

Há lacunas concretas:

- **R19a pode passar sem nascer cobrança:** não exige cobrança não nula e compara contra `getattr(..., None)`. (`app/chamados/tests/test_c20_fio_mudo.py:174`)
- **R19c não exerce adoção futura:** roda o vigia de hoje e consulta ausência amanhã, sem semear órfã futura nem abrir cobrança futura. (`app/chamados/tests/test_c20_fio_mudo.py:210`)
- O controle de tranca é anunciado, mas não há caso executável correspondente; também faltam segunda adoção após sucesso, trilha única, corrida C20, grupo respondido/duplicado e orçamento de queries. (`app/chamados/tests/test_c20_fio_mudo.py:11`, `app/chamados/tests/test_c20_fio_mudo.py:91`)

**R19d procede para dispensar questionário de quem nunca bateu:** o cartório carimba sem emitir, e adesão declara que a ação pertence ao supervisor/RH. Porém, a mesma lei manda agrupar os furos no pai próprio; ela não comprova legitimidade indefinida de qualquer filho diário vivo. R19d não verifica pai nem agrupamento. (`app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:1268`, `app/chamados/services/adesao.py:19`, `app/chamados/services/adesao.py:102`, `app/chamados/tests/test_c20_fio_mudo.py:106`)

Por fim, os números reproduzidos no teste somam **495**; retirados os três nunca-bateu, restam **492**, não o universo literal **494**. Reconciliação dos universos e comprovação individual das exclusões: **[não verificado]**. (`app/chamados/tests/test_c20_fio_mudo.py:1`, `app/chamados/tests/test_c20_fio_mudo.py:13`)

## 05/11 22:03 · sessao 107544328c40 · - · - · origem=Ronald (tmux)

Achados principais:

- **`reabrir()` não tem a mesma transação/trava de `abrir()`.** Lê estado sem `select_for_update`, grava estado em transação interna e só depois ressuscita/adota. A unidade estado+adoção depende de transação externa do chamador. `abrir()` garante `atomic` e trava do colaborador. (`app/chamados/models.py:937`, `app/chamados/models.py:940`, `app/chamados/models.py:1408`, `app/chamados/models.py:274`, `app/chamados/models.py:1477`)

- **Corrida/idempotência concorrente não garantida.** O helper coleta perguntas/âncoras antes de costurar; a porta trava o destino, mas não relê nem trava a pergunta antes de decidir. Duas execuções podem selecionar a mesma órfã e ambas salvar/registrar o evento; reprodução concorrente [não verificado]. (`app/chamados/models.py:464`, `app/chamados/models.py:473`, `app/chamados/models.py:496`, `app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:845`)

- **Continente fechado continua fechado.** O helper admite perguntas de qualquer disputa e não reabre a disputa; C6 pode considerá-las vivas, mas `_disputa_do_chamado` só retorna disputa aberta. Assim, existe contracaso estático de destino vivo com fio vivo, mas lookup administrativo ausente; ocorrência em produção [não verificado]. (`app/chamados/models.py:464`, `app/chamados/juizes.py:1135`, `app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:135`) A ressuscitação existente reabre o continente quando altera perguntas; a adoção não executa isso. (`app/chamados/models.py:1580`, `app/chamados/models.py:1607`)

Inventário:

| Campo/ato | Escritores existentes e utilização nova | Leitores relevantes |
|---|---|---|
| `chamado_destino` | `PerguntaDisputa.abrir` cria e reaponta identidade; `fio.reapontar_resolvedora` costura. Helper novo apenas chama a porta. (`models.py:2526`, `models.py:2552`, `services/fio.py:845`, `models.py:492`, todos em `app/chamados/`) | Âncora: `services/fio.py:116`; C6/memo: `juizes.py:817`; materialização: `juizes.py:743`; plano/gate: `models.py:344`; órfãs: `reconciliador.py:1480`. |
| `via_resolucao` | Núcleo `_escrever_carimbo`, portas `carimbar_identidade`/`descarimbar`/lote; exceções existentes `encerrar_por_fato_no_espelho` e `relavrar`. Novo dedup chama `retratar_duplicata` → `carimbar_identidade`. (`app/chamados/models.py:2632`, `app/chamados/models.py:2701`, `app/chamados/models.py:2722`, `app/chamados/models.py:2732`, `app/chamados/models.py:2749`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/fio.py:891`) | C6: `app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1171`; migrador: `app/chamados/services/fio.py:935`; ressuscitação: `app/chamados/models.py:1531`. |
| `validada_em` | Validação administrativa e devolução; simetria pela porta `carimbar`; desfazimento por resposta/correção/ressuscitação. Nenhum escritor direto novo. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/models.py:2668`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1580`) | C6: `app/chamados/juizes.py:1136`; estado: `app/chamados/models.py:2575`; ancoradas: `app/chamados/models.py:327`; reconciliação: `app/chamados/reconciliador.py:620`. |
| `status_local` | Núcleo `_gravar_estado`; nascimento pelas portas `abrir`/`criar`. Helper novo somente lê. (`app/chamados/models.py:222`, `app/chamados/models.py:1006`, `app/chamados/models.py:1131`, `app/chamados/models.py:452`, `app/chamados/models.py:469`) | Vigia: `app/chamados/services/vigia_fio.py:335`; gate: `app/chamados/models.py:400`; costura: `app/chamados/services/fio.py:764`; C6/materialização: `app/chamados/juizes.py:744`. |
| `pergunta_reatada` | Gate antigo e helper novo chamam `core.observ.evento`; antigo associa à âncora que fecha, novo ao destino que abre. (`app/chamados/models.py:423`, `app/chamados/models.py:496`) | Teste específico: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:302`; auditoria genérica: `app/core/views.py:705`. Consumidor de produção específico por nome [não verificado]. |
| Lavra | `veredito_lavrado.lavrar` atribui os três campos; `reapontar_resolvedora` salva destino e lavra juntos. (`app/chamados/veredito_lavrado.py:211`, `app/chamados/veredito_lavrado.py:227`, `app/chamados/services/fio.py:845`) | Cache/staleness: `app/chamados/veredito_lavrado.py:173`; C6 consulta o juiz de materialização: `app/chamados/juizes.py:1214`. |

**Não identifiquei segundo escritor direto introduzido pelo diff:** a lista autorizada do destino já contém `models.py` e `fio.py`, e a nova chamada usa `reapontar_resolvedora`. (`app/core/tests/test_contract_escritores_estado.py:60`, `app/chamados/models.py:492`)

**Travas:** `canonica_do_grupo` é cálculo em memória; competência compõe consultas; `retratar_duplicata` atualiza pergunta pela porta; `reapontar_resolvedora` trava chamado destino antes de atualizar a canônica. Não há nova trava explícita de `DisputaSupervisao` nesses helpers, portanto não há inversão direta nova de Disputa→Colaborador. (`app/chamados/services/fio.py:873`, `app/chamados/services/vigia_fio.py:834`, `app/chamados/services/fio.py:891`, `app/chamados/models.py:2720`, `app/chamados/services/fio.py:762`, `app/chamados/services/fio.py:847`) O hook da pergunta agenda becos somente em `on_commit`; o selo da ordem admite não enxergar inversões entre funções. (`app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:14`)

**Idempotência sequencial:** sucesso move a pergunta para `self`; segunda chamada marca o motivo em `_ja` e pula escrita/evento. Isso não prova “zero duplicata” global: uma órfã do mesmo motivo é ignorada quando já existe viva em `self`, sem retratá-la. (`app/chamados/models.py:477`, `app/chamados/models.py:484`) Duplicata respondida também pode permanecer viva pela precedência da resposta em C6. (`app/chamados/models.py:2704`, `app/chamados/juizes.py:1150`)

**“Uma linha” não é garantia absoluta:** `evento` é best-effort e pode retornar sem linha após falha; o teste novo não conta `LogAuditoria`. (`app/core/observ.py:98`, `app/core/observ.py:105`, `app/core/observ.py:139`, `app/chamados/tests/test_c20_fio_mudo.py:1`)

## 05/11 22:03 · sessao 7fd15dd1c688 · - · - · origem=Ronald (tmux)

Achados estáticos principais:

- **Pouso antes do pré-requisito:** o método retrata as duplicatas antes de saber se a canônica pode ser costurada; se a porta recusar, mantém as retratações e segue a abertura (`app/chamados/models.py:487-495`; `app/chamados/services/fio.py:837-849`). Contraexemplo possível: célula em D, contexto em D+2; `dia_do_fato` escolhe D pela célula, mas E4-c compara o contexto e recusa D×D+2 (`app/chamados/juizes.py:90-92`, `:1973-1999`). Execução desse exemplo: [não verificado].
- **Dedup incompleto:** havendo viva já ancorada no destino, `_ja` pula todo o grupo órfão; não aplica canônica nem retrata a irmã (`app/chamados/models.py:477-490`). A migração existente compara origem+destino e aplica a canônica ao grupo inteiro (`app/chamados/services/fio.py:937-945`).
- **Nenhuma cópia evidente de juiz:** dia, motivo, competência, viva e âncora são delegados às funções existentes (`app/chamados/models.py:454-475`); registro C1/C5/C6/C8/C14/C15 em `app/core/juizes.py:260-277`. `_plano_pergunta_viva` parte das perguntas ancoradas e procura sobrevivente pelo C8/E4-c; o método novo parte da cobrança e procura perguntas do colab-dia-motivo (`app/chamados/models.py:364-377`, `:464-475`). Igualdade de data limita a busca, enquanto E4-c permite D±1 da mesma jornada overnight (`app/chamados/juizes.py:1957-1959`, `:1991-1997`); conformidade dessa limitação com o aval: [não verificado].

Contraexemplos e controles:

- Outro módulo de **destino novo** é recusado pela constante; outro módulo da **âncora antiga** não é recusado, desde que terminal (`app/chamados/models.py:432`, `:452`, `:473`).
- Outro dia/motivo fica fora do filtro; morta pelo C6 fica fora; competência entregue fica fora (`app/chamados/models.py:460-476`).
- Agrupado sob pai vivo é recusado antes da reabertura, e o `abrir` também deduplica filho agrupado (`app/chamados/models.py:1387-1388`, `:1442-1443`, `:960-967`). O helper isolado não pergunta por agrupamento (`app/chamados/models.py:452-492`).
- `nunca_bateu` pertence aos vereditos que acusam e pode manter uma pergunta existente viva pelo C6 (`app/escala/models.py:1767-1768`; `app/chamados/juizes.py:1181-1184`). Assim, órfã prévia pode ser adotada nesse cenário; o método não tem exclusão própria de adesão (`app/chamados/models.py:464-492`). Isso tensiona “não há o que responder pelo questionário” de `app/chamados/services/adesao.py:19-21`, mas proibir criar pergunta nova não implica automaticamente proibir reatar uma existente: [não verificado].
- Pergunta viva em disputa fechada pode ser adotada porque o filtro não exige disputa aberta (`app/chamados/models.py:464-465`); o app continua filtrando disputas abertas (`app/api/views.py:1008`), enquanto o fio do admin deliberadamente mostra vivas em disputas fechadas (`app/chamados/services/fio.py:209-221`). Portanto “reatada” não prova retorno ao questionário do colaborador; reabrir essa disputa automaticamente contrariaria o precedente documentado em `app/chamados/services/fio.py:211-212`.
- Competência entregue: exclusão herdada de `_vivas_entre`, não novo juiz (`app/chamados/models.py:347`, `:460`). Porém o renascimento declara expressamente permitido em competência exportada e chama `reabrir` (`app/chamados/models.py:1030-1035`, `:1055`): pode renascer sem adotar órfã. A resolução da tensão com L-113/L-092 exige distinguir escopo de cura e lei; conformidade final: [não verificado].

Cinco atos:

1. **Reabrir:** coberto para Chamado pelo novo gancho, após ressuscitar próprias (`app/chamados/models.py:1477-1478`); renascimento passa por ele (`:1055`). Reabrir Disputa diretamente não chama adoção (`:2116-2171`, `:2218-2224`); a casca administrativa chama `ch.reabrir` somente se encerrado (`app/chamados/services/acoes_disputa.py:265-266`).
2. **Retratar duplicata:** porta preserva identidade/canônica, mas não reata a canônica (`app/chamados/services/fio.py:878-895`). Pode matar a última pergunta de um chamado vivo enquanto a canônica continua num terminal; o gancho novo não roda nessa operação (`app/chamados/models.py:1007-1009`, `:1478`). `abrir` sobre existente vivo retorna sem adoção (`:956-959`).
3. **Validação:** já atomiza carimbo, materialização e devolução de recusa e recalcula por evento (`app/chamados/services/validacao.py:395-427`). Validada morre legitimamente pelo C6 (`app/chamados/juizes.py:1136-1137`); essa morte não exige pergunta nova.
4. **Morte por leitura:** C6 já decide tranca, marco e célula (`app/chamados/juizes.py:1138-1146`, `:1177-1196`). E2 carimba absolvidas, consulta C6 e poupa trancadas (`app/chamados/reconciliador.py:582-604`, `:651-658`), acionado pelo cartório (`app/ponto/services/cartorio.py:958`). Morte legítima sai do universo; não demonstra lacuna por si.
5. **Reapontar:** já protege destino pela validação E4-c, trava, lavra e save conjunto (`app/chamados/services/fio.py:837-849`). Não verifica o chamado de origem que perdeu a última pergunta; preservar o fio da origem não é efeito garantido dessa porta (`:784`, `:845-848`). Se a transferência torna a origem legítima sem pergunta, essa exclusão precisa de prova própria: [não verificado].

_2021 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
