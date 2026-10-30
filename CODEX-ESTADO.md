# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 07:39** · publicado em **30/10/2026 07:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 07:39 · sessao af28e22338ae · O261-N · sonda · origem=Code

**1. Autoria e horários**

- **Quem criou P#51789–51792 e quando: [nao verificado].** A sonda informa abertura da **disputa** em 28/10 06:34:43 e julgamento das perguntas em 30/10 06:35:14; não informa emissor nem nascimento das perguntas [nao verificado]. `aberta_em` pertence à disputa (`app/chamados/models.py:1331`), uma disputa existente pode receber perguntas novas (`app/chamados/services/disputa_emissao.py:618`) e `julgado_em` recebe o instante da lavra do veredito (`app/chamados/veredito_lavrado.py:228`).
- O escritor de nascimento é `PerguntaDisputa.abrir`, chamado por `gerar_perguntas_disputa`; a criação efetiva ocorre em `app/chamados/models.py:2085`, acionada em `app/chamados/services/disputa_emissao.py:389`. Isso identifica a porta do código, **não o emissor efetivo dessas quatro perguntas [nao verificado]**.
- **#35679:** a transcrição registra criação em **29/10 09:00:12**, resolução em **30/10 06:35:14**, via **`sistema`**, `respondido_por=None` e `superado_por=None` [nao verificado — dados da transcrição, sem arquivo numerado]. No código, `reconciliar()` passa `EVENTO_VIA['RECONCILIAR']` ao núcleo (`app/chamados/models.py:444`), cujo valor é `sistema` (`app/chamados/catalogo/motor.py:95`). **O escritor de `via_encerramento` é `app/chamados/models.py:209`**, persistido em `:231`; `resolvido_em` é carimbado em `:208`.
- **Qual serviço/cron chamou esse encerramento e por qual motivo concreto: [nao verificado].** A via identifica o caminho de reconciliação, mas a transcrição não contém a chamada nem sua trilha. O censo seleciona chamados vivos (`app/chamados/services/vigia_fio.py:186`); atribuir a saída da célula a uma rotina específica continua [nao verificado].

**2. Emissão para `nunca_bateu`**

**O emissor efetivo e até o `modulo_origem` de #35679 não aparecem na sonda [nao verificado].** Para o módulo `batida_ausente`, o código contém estes caminhos:

| Caminho | Escritor/delegação e passagem pelo veto |
|---|---|
| `detectar_ausencias::_abrir_chamado` | Abre em `app/ponto/management/commands/detectar_ausencias.py:688`; consulta o veto em `:626` e retorna em `:628`. |
| Cartório | Delega ao funil acima em `app/ponto/services/cartorio.py:1358` e `:1394`; sem adesão, retorna antes, em `:1003`. |
| `emitir_furo_retroativo` | Delega ao mesmo funil em `app/ponto/management/commands/emitir_furo_retroativo.py:142`; passa pelo veto desse funil. |
| `vigia_de_hora` | Delega ao cartório por `julgar_celula`, em `app/ponto/services/vigia_de_hora.py:130`. |
| `reconciliar_grade::_emitir_furo` | Abre em `app/ponto/management/commands/reconciliar_grade.py:499`; consulta o veto em `:494` e retorna em `:496`. |
| Reconciliador c2, `reemitir_chamado_do_dia` | Abre em `app/chamados/reconciliador.py:862`; consulta o veto em `:838` e retorna em `:842`. |
| Abertura por formulário/request | Aceita módulo catalogado em `app/chamados/views.py:170`, `:174`; delega em `:197`; cria com o módulo recebido em `app/chamados/services/abertura.py:47`, `:56`. **Não passa pelos dois vetos citados** (`app/chamados/services/abertura.py:19`; `app/chamados/models.py:805`). |

`nunca_bateu` está literalmente em `VEREDITOS_QUE_VETAM` (`app/ponto/marcos.py:119`), aplicado pela leitura da célula em `:155` e pelo retorno em `:161`. Portanto, **os caminhos automáticos acima não podem emitir se essa consulta encontra `nunca_bateu`**; o cartório também interrompe seu corpo quando `adesao is None` (`app/ponto/services/cartorio.py:907`, `:1003`).

A request passa por outro gate de absolvição (`app/chamados/models.py:821`, `:839`), que não equivale àquele veto: ele não absolve vereditos pertencentes a `VEREDITOS_ACUSAM` (`app/chamados/reconciliador.py:975`), conjunto que inclui `nunca_bateu` (`app/escala/models.py:1735`). Isso não prova emissão da request para a célula 143031 [nao verificado].

**3. Efeito real de faltar `out['cods']`**

O ramo sem adesão lavra/carimba e retorna sem registrar a data (`app/ponto/services/cartorio.py:981`, `:999`, `:1003`). A gravação do ramo normal usa **somente a data** como chave, inclusive quando `cods` é lista vazia (`:1120`); a fábrica é chamada pelo teste `_cel.data in _julgadas` (`:759`, `:769`, `:770`).

| Chamador | Consequência |
|---|---|
| Lote `processar_cartorio` | **Não impede sempre.** Cria um único `out` antes do laço e o reutiliza entre colaboradores (`app/ponto/management/commands/processar_cartorio.py:43`, `:59`, `:65`). Se outro colaborador anterior já registrou a data, a célula sem adesão alcança `perguntar_pela_celula`; sem essa entrada, não alcança (`app/ponto/services/cartorio.py:769`, `:1120`). |
| `julgar_celula` | Cria `out` novo para uma célula (`app/ponto/services/cartorio.py:1559`, `:1560`). Nesse ramo, não surge entrada em `cods`, e a fábrica não é chamada (`:1003`, `:759`, `:769`). |
| `rejulgar_furos_do_vinculo` | Cria `out` novo por invocação; se alcançar o ramo sem adesão, ocorre o mesmo bloqueio (`app/escala/regua_defesa.py:691`, `:692`; `app/ponto/services/cartorio.py:1003`, `:769`). |
| `celula_veredito_velho` | Passa `contadores()` novo por colaborador; mesmo bloqueio nesse ramo (`app/ponto/management/commands/celula_veredito_velho.py:139`, `:144`, `:145`). |
| Script `rejulgar_o118_col221` | Usa `out` novo para um colaborador; mesmo efeito se alcançar esse ramo com aplicação habilitada (`bin/rejulgar_o118_col221.py:20`, `:23`, `:25`). |

Alcançar a fábrica não garante pergunta nova: ela ainda verifica trabalho previsto, situação, data, vencimentos e deduplicação (`app/chamados/services/fabrica_da_celula.py:78`, `:80`, `:85`, `:106`, `:109`). **Não é possível atribuir as perguntas existentes da célula 143031 à colisão de datas do lote [nao verificado].**

Os outros usos são:

- **Cluster:** o wrapper testa apenas se `_julgadas` está não vazio e chama `_cluster(c, hoje)`; não entrega os códigos ao serviço (`app/ponto/services/cartorio.py:763`, `:767`). Assim, entradas de colaborador anterior também satisfazem essa condição; o cluster lê a ATA do colaborador atual (`app/ponto/services/cluster_espurio.py:22`, `:23`).
- **Releitura:** o gatilho é o aumento de `emitidos`, não `cods` (`app/ponto/services/cartorio.py:743`, `:798`). Cria acumulador novo, executa o corpo e mescla seus códigos ao acumulado (`:810`, `:811`, `:812`), sem nova fábrica depois (`:823`). O ramo sem adesão retorna antes das emissões (`:1003`, `:1358`, `:1394`).

**4. FROTA**

- **Células `nunca_bateu` sem pergunta nenhuma: 0**, conforme `nb_sem_pergunta_nenhuma=0` [nao verificado — transcrição fornecida].
- **Chamados vivos em células `nunca_bateu`: 12**, nos módulos `ausencia_colab`, `batida_ausente`, `disputa_supervisao` e `sem_previsao_escala` [nao verificado — transcrição fornecida].
- **Quantidade de células distintas com chamado vivo: [nao verificado].** O campo fornecido conta chamados; não informa a contagem distinta de células [nao verificado].

## 30/10 07:39 · sessao 74c8c0ee326f · O261-V · sonda · origem=Code

**1. Célula 129745**

- **P46703, resposta 19:00:** materialização anterior por 666 e validação posterior por 657 são a assinatura compatível com `_carimbar_validacao`, que altera somente `validada_em/por`. Os outros escritores positivos também gravam materialização no ato. A execução histórica exclusiva permanece **[não verificado]**. (`app/chamados/services/validacao.py:127-133`; `app/chamados/services/materializacao.py:506-530`, `1148-1198`; `app/chamados/services/disputa_emissao.py:1561-1577`)
- **Lastro de P46703:** a lista fornecida não mostra batida em **21/10 19:00**; `materializada_em` sozinho não demonstra plantio. Ausência absoluta de lastro: **[não verificado]**, pois faltam os vínculos das batidas com a pergunta, usados pelo juiz. (`app/chamados/juizes.py:674-681`, `730-732`)
- **P46704, resposta 07:00:** existe correspondência horária em **B140649, 22/10 07:00, não retratada**, segundo a sonda **[não verificado — sem confirmação independente]**. Escritor exclusivo da validação: **[não verificado]**; validação seguida de materialização e carimbo de simetria produzem estados compatíveis. (`app/chamados/services/validacao.py:389`, `415-417`; `app/chamados/services/materializacao.py:506-530`, `1194-1198`)
- **CH33286 cobra E19:00:** o lastro horário de S07:00 não satisfaz essa cobrança; o fechador exige os marcos cobrados acesos. Dono da contradição **fio morto: ESTRUTURA**; dono específico do furo: **[não verificado]**. (`app/ponto/services/lastro.py:170-181`; `app/chamados/services/vigia_fio.py:9-14`; `app/ponto/management/commands/e6_oraculo.py:90-100`; marco cobrado: **[não verificado — sonda]**)

**2. Célula 129747**

- **P49422, resposta 19:00:** mesma assinatura de validação posterior à materialização, compatível com `_carimbar_validacao`; execução histórica exclusiva **[não verificado]**. (`app/chamados/services/validacao.py:127-133`)
- **Lastro de P49422:** nenhuma batida em **23/10 19:00** aparece na lista fornecida. Plantio vinculado ou ausência absoluta de lastro: **[não verificado]**. (`app/chamados/juizes.py:674-681`, `730-732`; conteúdo da lista: **[não verificado — sonda]**)
- **P49423, resposta 07:00:** correspondência horária em **B140652, 24/10 07:00, não retratada** **[não verificado — sonda]**. Escritor exclusivo da validação **[não verificado]**, pelos mesmos caminhos concorrentes de validação e simetria. (`app/chamados/services/validacao.py:389`, `415-417`; `app/chamados/services/materializacao.py:506-530`, `1194-1198`)
- **CH34254 cobra E19:00:** S07:00 não comprova atendimento desse marco. Dono do fio morto: **ESTRUTURA**; dono específico do furo: **[não verificado]**. (`app/ponto/services/lastro.py:170-181`; `app/chamados/services/vigia_fio.py:9-14`; marco cobrado: **[não verificado — sonda]**)

**3. Célula 129749**

- **P50179, resposta 19:00:** assinatura compatível com `_carimbar_validacao` sobre materialização anterior; execução histórica exclusiva **[não verificado]**. (`app/chamados/services/validacao.py:127-133`)
- **Lastro de P50179:** nenhuma batida em **25/10 19:00** aparece na lista fornecida. Plantio vinculado ou ausência absoluta de lastro: **[não verificado]**. (`app/chamados/juizes.py:674-681`, `730-732`; conteúdo da lista: **[não verificado — sonda]**)
- **P50180, resposta 07:00:** correspondência horária em **B140653, 26/10 07:00, não retratada** **[não verificado — sonda]**. Escritor exclusivo da validação **[não verificado]**: a igualdade mostrada até segundos não distingue validação seguida de materialização dos carimbos de simetria. (`app/chamados/services/validacao.py:389`, `415-417`; `app/chamados/services/materializacao.py:506-530`, `1194-1198`)
- **CH34850 cobra E19:00:** S07:00 não comprova atendimento desse marco. Dono do fio morto: **ESTRUTURA**; dono específico do furo: **[não verificado]**. (`app/ponto/services/lastro.py:170-181`; `app/chamados/services/vigia_fio.py:9-14`; marco cobrado: **[não verificado — sonda]**)

**4. Célula 129751**

- **P50883, resposta 19:00:** assinatura compatível com `_carimbar_validacao` sobre materialização anterior; execução histórica exclusiva **[não verificado]**. (`app/chamados/services/validacao.py:127-133`)
- **Lastro de P50883:** nenhuma batida em **27/10 19:00** aparece na lista fornecida. Plantio vinculado ou ausência absoluta de lastro: **[não verificado]**. (`app/chamados/juizes.py:674-681`, `730-732`; conteúdo da lista: **[não verificado — sonda]**)
- **P51025, resposta 07:00:** correspondência horária em **B140654, 28/10 07:00, não retratada** **[não verificado — sonda]**. Escritor exclusivo da validação **[não verificado]**, pelos caminhos distintos já identificados. (`app/chamados/services/validacao.py:389`, `415-417`; `app/chamados/services/materializacao.py:506-530`, `1194-1198`)
- **CH35331 cobra E19:00:** S07:00 não comprova atendimento desse marco. Dono do fio morto: **ESTRUTURA**; dono específico do furo: **[não verificado]**. (`app/ponto/services/lastro.py:170-181`; `app/chamados/services/vigia_fio.py:9-14`; marco cobrado: **[não verificado — sonda]**)

**5. Célula 140114**

- **P50169, resposta 17:30:** assinatura compatível com `_carimbar_validacao` sobre materialização anterior; execução histórica exclusiva **[não verificado]**. (`app/chamados/services/validacao.py:127-133`)
- **Tem lastro horário no dado:** **B137026, 25/10 17:30:12, não retratada** **[não verificado — sonda]**. Isso não comprova atendimento de **E21:30**, cobrada por CH35101, nem o vínculo com a pergunta, ausente na sonda. A cobrança é julgada pelo próprio marco. (`app/ponto/services/lastro.py:170-181`; `app/chamados/juizes.py:674-681`; marco cobrado: **[não verificado — sonda]**)
- **P50170:** não há validação gravada no estado apresentado **[não verificado — sonda]**. A cláusula exata que produziu `viva=False` é **[não verificado]**: `fantasma` não pertence às vias que encerram por identidade. Antes do retorno obrigatório de pergunta viva para célula acusando, os encerramentos compatíveis com os campos mostrados são **tranca por leitura**, em `1010-1011`, ou **marco próprio aceso na ATA**, em `1050-1052`. (`app/chamados/juizes.py:1005-1057`)
- Para `orfao_14h`, o marco próprio é **S/hf**; sua prova lê `cel.ata.lampadas[].acesa`. A sonda apresenta `dna.lampadas`, sem essa ATA e sem a tranca: não permite escolher a cláusula. Dono do fio morto: **ESTRUTURA**; dono específico do furo: **[não verificado]**. (`app/chamados/juizes.py:823-828`, `866-872`; `app/chamados/services/vigia_fio.py:9-14`)

**Classes e pontos de decisão**

**Validação sobre materialização anterior.** O caminho concreto da cópia carimba a validação em `validacao.py:389`, mas só chama a unidade de materialização se `materializada_em` estiver nulo, em `415-417`. O lote posterior também exige materialização nula, e a devolução da validação não atua quando já existe materialização. Esse caminho permite conservar validação sem reapurar a unidade; identificar sua execução histórica em cada pergunta é **[não verificado]**. (`app/chamados/services/validacao.py:182-183`, `389`, `415-417`; `app/chamados/services/materializacao.py:1286-1290`)

**Pergunta morta não equivale a cobrança satisfeita.** `pergunta_viva` retorna `False` imediatamente com `validada_em` preenchido. Já `lastro` mantém `furo_real` quando a lâmpada cobrada está apagada e só encaminha `com_lastro` ao `reconciliar()`. Portanto, as respostas 07:00/17:30 com correspondência horária não demonstram falha do fechador das entradas 19:00/21:30. (`app/chamados/juizes.py:1006-1007`; `app/ponto/services/lastro.py:265-272`, `306-317`)

**Rejulgamento e fechamento.** O evento de batida aciona o cartório; o cartório monta a ATA atual e tenta reconciliar quando todos os marcos da cobrança acenderam. O reconciliador por absolvição da célula retorna sem fechar quando ela ainda acusa. Qual desses caminhos efetivamente rodou, recusou ou falhou nos atos apresentados — a solicitada “linha em que não fez” — é **[não verificado]**. (`app/ponto/signals.py:108-135`; `app/ponto/services/cartorio.py:1148-1163`; `app/chamados/reconciliador.py:975-976`)

**Dono L-099.** O fio morto é declarado contradição estrutural; isso não autoriza classificar automaticamente a causa do furo. O juiz exige flags de cadastro, batida fora de turno, paridade das batidas **atribuídas ao dia** e pertencimento à lista `dias_cadastro_x_realidade`. Esses resultados não constam da sonda; contar as cinco/seis batidas da janela vizinha não reproduz esse julgamento. Assim, atribuir definitivamente os furos a CADASTRO ou BATIDA seria **[não verificado]**. (`CLAUDE.md:306-316`; `app/ponto/management/commands/e6_oraculo.py:90-100`; `app/chamados/services/vigia_fio.py:99-101`)

## 30/10 07:39 · sessao 56120f37885d · - · - · origem=Ronald (tmux)

Inventário de emissores de `batida_ausente`:

| Caminho | Escritor | Veto `nunca_bateu` |
|---|---|---|
| `detectar_ausencias::_abrir_chamado` | `app/ponto/management/commands/detectar_ausencias.py:688` (módulo definido em `:29`) | Chama `celula_veta_cobranca` e retorna em `:625-628`. |
| Cartório | Delega ao escritor acima em `app/ponto/services/cartorio.py:1358` e `:1394`. | Além do veto do escritor, `adesao is None` retorna em `:1003`, antes dessas emissões; consulta de adesão em `:899-907`. |
| `emitir_furo_retroativo` | Delega ao escritor acima em `app/ponto/management/commands/emitir_furo_retroativo.py:142`. | Passa pelo mesmo veto; também pula colaborador sem batidas na janela em `:52-55`. |
| `reconciliar_grade::_emitir_furo` | `app/ponto/management/commands/reconciliar_grade.py:499` (CANON em `:54`) | Chama o veto e retorna em `:494-496`. |
| Reconciliador c2 `reemitir_chamado_do_dia` | `app/chamados/reconciliador.py:862` (chave do módulo em `:804`) | Chama o veto e retorna em `:838-842`. |
| Abertura pela request | `app/chamados/views.py:170-175` aceita o módulo do catálogo; delega em `:196-207`; escritor `app/chamados/services/abertura.py:47-65`. | Não passa pelo veto `celula_veta_cobranca` nem pelo ramo de adesão; segue `ChamadoColaborador.criar`, em `app/chamados/models.py:805-834`. |

`vigia_de_hora` chega ao cartório por `julgar_celula` em `app/ponto/services/vigia_de_hora.py:130`.

O veto compartilhado contém literalmente `nunca_bateu` em `app/ponto/marcos.py:119` e o aplica em `:155-162`.

A abertura pela request passa por outro gate, `_celula_que_absolve` (`app/chamados/models.py:821-825`, implementação em `:839-851`). Esse gate não equivale a `VEREDITOS_QUE_VETAM`: `_absolvido_pela_celula` retorna `None` quando o veredito pertence a `VEREDITOS_ACUSAM` (`app/chamados/reconciliador.py:975`).

As chaves de contexto de #35679 coincidem com o formato produzido pelo funil `detectar_ausencias`: base em `app/ponto/management/commands/detectar_ausencias.py:700-706`; `folga_nao_declarada` e `nao_calado_porque` em `:753-756`; `agrupado_em` possui escritor em `app/chamados/services/limbo_folgas.py:219`. Porém a sonda não traz `modulo_origem`, autor, trilha nem emissor: atribuir exatamente #35679 a uma execução desse funil continua **[nao verificado]**.

## 30/10 07:38 · sessao ae6aaf07e1c7 · - · - · origem=Ronald (tmux)

Leitura somente; nenhum arquivo alterado. [não verificado]

- **Lastro da resposta não equivale a lastro da cobrança.** Os quatro chamados de col451 cobram **E 19:00**; as batidas retro **07:00 do dia seguinte** sustentam temporalmente as respostas P46704/P49423/P50180/P51025, mas não demonstram atendimento da entrada cobrada. A autoridade do fechamento exige todos os marcos nomeados acesos, por hora **e tipo**; lâmpada apagada mantém `furo_real`. (`app/ponto/services/lastro.py:170`, `app/ponto/services/lastro.py:175`, `app/ponto/services/lastro.py:179`, `app/ponto/services/lastro.py:265`, `app/ponto/services/lastro.py:271`; correspondência às batidas da sonda: [não verificado])

- **140114:** B137026, de 25/10 17:30:12 e não retratada, sustenta temporalmente a resposta `17:30` de P50169; o chamado CH35101 cobra **E 21:30**, portanto aquela presença não prova regularização do marco cobrado. (`app/ponto/services/lastro.py:175`, `app/ponto/services/lastro.py:179`; presença e estado da batida, extraídos da sonda sem arquivo:linha: [não verificado])

- **Limite da prova técnica de materialização:** a sonda não informa `tipo_resposta`, `data_evento`, `tipo` das batidas, `pergunta_origem` nem `ata`. O juiz usa esses campos: para hora/data_hora procura plantio vinculado e vivo; aceita plantio em data_evento −1/0/+1 com hora dentro de ±20 minutos; na prova externa exige tipo e instante do slot. Logo não é rigoroso afirmar ausência absoluta de lastro dos P19 apenas porque inexiste B19 no próprio dia — há B19 vizinhas, mas o vínculo necessário não foi mostrado. (`app/chamados/juizes.py:225`, `app/chamados/juizes.py:234`, `app/chamados/juizes.py:257`, `app/chamados/juizes.py:263`, `app/chamados/juizes.py:266`, `app/chamados/juizes.py:674`, `app/chamados/juizes.py:676`, `app/chamados/juizes.py:680`, `app/chamados/juizes.py:730`)

- **Quem rejulga/fecha:** `post_save(Batida)` chama cartório para dia civil da marca e véspera; cartório monta a ata atual e só tenta reconciliar a cobrança por marco quando `_acesos(...) is True`. A rede `lastro.fechar` só chama `reconciliar()` para `com_lastro`. A materialização tenta fechar destino/clones, mas passa pelo mesmo `reconciliar()` e pelo bloqueio de irmã sem consumo. (`app/ponto/signals.py:108`, `app/ponto/signals.py:112`, `app/ponto/signals.py:132`, `app/ponto/signals.py:135`, `app/ponto/signals.py:93`, `app/ponto/services/cartorio.py:1148`, `app/ponto/services/cartorio.py:1163`, `app/ponto/services/lastro.py:306`, `app/ponto/services/lastro.py:317`, `app/chamados/services/materializacao.py:702`, `app/chamados/services/materializacao.py:711`, `app/chamados/services/materializacao.py:919`)

- **Por que a cópia permite chamado vivo com perguntas validadas:** fechar a pergunta não é a condição de fechamento do chamado. `reconciliar()` consulta a premissa; o gate recusa furo ainda vivo para a folha, salvo regularização/agrupamento ou deduplicação sob outra cobrança viva. A regularização pode decorrer do marco cobrado aceso; não decorre simplesmente de `validada_em`. (`app/chamados/models.py:440`, `app/chamados/models.py:470`, `app/chamados/models.py:495`, `app/chamados/models.py:498`, `app/chamados/models.py:525`, `app/chamados/models.py:957`, `app/chamados/models.py:959`)

- **Reconciliador não absolve estas células pelo veredito fornecido:** `_absolvido_pela_celula` retorna `None` quando o veredito pertence a `VEREDITOS_ACUSAM`. Ele não transforma uma validação individual em absolvição do dia. (`app/chamados/reconciliador.py:948`, `app/chamados/reconciliador.py:975`, `app/chamados/reconciliador.py:976`)

- **“Em que linha não fez” como execução histórica:** [não verificado]. Sem log, a sonda não mostra se houve signal, tentativa, exceção, decisão anterior ou recusa. É possível apontar as condições acima; não é possível afirmar qual linha foi efetivamente percorrida e recusou em cada ato. A própria porta do signal contém retorno por julgamento em curso e tratamento de exceção. (`app/ponto/signals.py:88`, `app/ponto/signals.py:90`, `app/ponto/signals.py:91`)

- **Dono formal L-099:** não pode ser determinado exatamente por esta sonda. O juiz recebe flags de cadastro, batida fora do turno, horas atribuídas pelo oráculo e pertencimento à lista `dias_cadastro_x_realidade`; faltam esses insumos. O código dá precedência: cadastro incompleto/sem vínculo → CADASTRO; batida fora do turno → BATIDA; horas em contagem ímpar → BATIDA; depois lista CxR → CADASTRO; resíduo → ESTRUTURA. DNA e protesto sozinhos não substituem esses insumos. (`CLAUDE.md:311`, `CLAUDE.md:313`, `app/ponto/management/commands/e6_oraculo.py:90`, `app/ponto/management/commands/e6_oraculo.py:96`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:100`, `app/ponto/management/commands/e6_oraculo.py:126`, `app/ponto/management/commands/e6_oraculo.py:142`, `app/ponto/management/commands/e6_oraculo.py:149`, `app/ponto/management/commands/e6_oraculo.py:155`)

- **Não converter classe de fio morto em dono:** o próprio vigia declara que essa classificação pertence ao juiz L-099, e não a um mapa de classes da porta. (`app/chamados/services/vigia_fio.py:99`, `app/chamados/services/vigia_fio.py:100`, `app/chamados/services/vigia_fio.py:101`)

## 30/10 07:38 · sessao 376360c5b485 · - · - · origem=Ronald (tmux)

Atribuição estrita:

- **Emissor e instante de criação das P#51789–51792: [nao verificado].** A sonda mostra abertura da disputa, não nascimento das perguntas: `DisputaSupervisao.aberta_em` é `auto_now_add` em `app/chamados/models.py:1331`. Uma disputa existente recebe novas perguntas em `app/chamados/services/disputa_emissao.py:618`. `julgado_em` é escrito como instante da lavra do cache em `app/chamados/veredito_lavrado.py:228`; portanto 30/10 06:35:14 não prova criação.
- **Escritor genérico de perguntas:** `gerar_perguntas_disputa` chama `PerguntaDisputa.abrir` em `app/chamados/services/disputa_emissao.py:389`; a criação efetiva ocorre em `app/chamados/models.py:2085`, e seu log `pergunta_emitida` em `app/chamados/models.py:2092`. O gate também reaponta perguntas já existentes e retorna sem criá-las em `app/chamados/models.py:2058` e `app/chamados/models.py:2084`; logo a disputa atualmente apresentada não identifica emissor original.
- **#35679:** a sonda declara criado 29/10 09:00:12 e resolvido 30/10 06:35:14, via `sistema`, sem respondente e sem superador — dados fornecidos sem arquivo:linha, portanto **[nao verificado] documentalmente**. O caminho correspondente no código é `ChamadoColaborador.reconciliar`, que chama `_gravar_estado(..., via=EVENTO_VIA['RECONCILIAR'])` em `app/chamados/models.py:444`; `RECONCILIAR → sistema` está em `app/chamados/catalogo/motor.py:95`. O escritor de `via_encerramento` é `app/chamados/models.py:209`, com persistência em `app/chamados/models.py:231`; `resolvido_em` é carimbado em `app/chamados/models.py:208`.
- **Qual serviço/cron chamou esse encerramento e sua causa concreta: [nao verificado].** Existem múltiplos chamadores, por exemplo `app/chamados/reconciliador.py:525` e `app/ponto/services/cartorio.py:1163`, `:1175`, `:1196`, `:1266`; horário e via não distinguem esses chamadores.
- O fechamento pode relavrar perguntas apontadas para o chamado: `app/chamados/models.py:243`–`:249`. Isso explica a existência de um caminho de código entre encerramento e `julgado_em`, mas não prova que tenha sido esse o caminho das quatro perguntas da sonda **[nao verificado]**.
- `contexto_keys` sozinho não prova módulo nem emissor de #35679 **[nao verificado]**. Não atribuir ao detector apenas porque contém `horario_previsto`, `atraso_min`, `marcos_faltantes` ou ressalvas de folga.

## 30/10 07:38 · sessao c98b4e89e71a · - · - · origem=Ronald (tmux)

Levantamento somente leitura dos escritores:

| Perguntas da sonda | Atribuição sustentada pela assinatura |
|---|---|
| P46703, P49422, P50179, P50883, P50169 | `_carimbar_validacao` é o escritor positivo que grava somente `validada_em/por`, preservando materialização anterior: `app/chamados/services/validacao.py:127-133`. Os outros três escritores positivos gravam também materialização com o mesmo instante/autor da validação, incompatível com a assinatura atual `mat` anterior/666 e validação posterior/657 em uma única dessas escritas: `materializacao.py:506-530`, `materializacao.py:1148-1198`, `disputa_emissao.py:1561-1583`. Execução histórica exclusiva, sem trilha: [não verificado]. |
| P46704, P49423, P50180, P51025 | Compatíveis com validação seguida de materialização: `validacao.py:389`, `validacao.py:415-417`, `materializacao.py:1148-1198`. A sonda mostra segundos, portanto igualdade dos microssegundos [não verificado]. Também não exclui só pelo estado o carimbo de simetria de `_materializar_ausencia_de_resposta`, que permite `via=declaracao` quando a irmã declarou hora: `materializacao.py:506-530`. Autor histórico exclusivo de `validada_em`: [não verificado]. |
| P50170 | `validada_em/por` são nulos na sonda: ninguém identificado como escritor de validação; eventual validação passada posteriormente apagada [não verificado]. |

O caminho concreto que permite validar uma pergunta já materializada sem reapurar o lastro é:

- A validação carimba antes da tentativa de materializar: `app/chamados/services/validacao.py:388-389`.
- A unidade só roda se `materializada_em is None`: `app/chamados/services/validacao.py:415-417`.
- A devolução recusa atuar se materialização já existe: `app/chamados/services/validacao.py:180-185`.
- O lote posterior também só seleciona materialização nula: `app/chamados/services/materializacao.py:1286-1290`.

Portanto, o carimbo antigo `fantasma` explica pelo código a preservação da validação sem nova verificação da unidade; afirmar que isso foi precisamente o caminho histórico de cada registro exige trilha ausente: [não verificado].

A materialização automática por chamado resolvido não é uma atribuição equivalente para os cinco fantasmas: ela seleciona materialização nula e, quando carimba validação, também substitui materialização/autor pelo mesmo `agora/user`: `app/chamados/services/disputa_emissao.py:1536-1539`, `app/chamados/services/disputa_emissao.py:1561-1577`.

Cuidados para a consolidação:

- P50169 tem lastro horário visível na sonda: B137026, 25/10 17:30:12, não retratada, para resposta 17:30; tipo, aprovação e vínculo com a pergunta [não verificado].
- O juiz `_pergunta_materializada_de_fato` avalia `via` sem testemunha antes do ramo de slot e pode reprovar `fantasma` pela lei do dia: `app/chamados/juizes.py:603-623`; logo reprovação desse juiz não prova ausência física da batida respondida.
- As quatro respostas 07:00 têm batidas retroativas visíveis no horário do encerramento noturno na sonda; vínculo `pergunta_origem` [não verificado]. O ramo que cria `origem='disputa_s84_retro'` está em `app/chamados/services/materializacao.py:871-879`.
- A regra L-099 determina ESTRUTURA quando o sistema se contradiz dado o cadastro, CADASTRO quando DNA não descreve batidas, BATIDA quando falta/sobra batida: `CLAUDE.md:306-316`.

## 30/10 07:38 · sessao a46ba23aaa09 · - · - · origem=Ronald (tmux)

O ramo `adesao is None` não registra `out['cods']`: incrementa `nunca_bateu`, lavra/carimba e retorna antes da única gravação `out.setdefault('cods', {})[data] = list(cods)` do ramo normal. (`app/ponto/services/cartorio.py:907`, `:946`, `:981`, `:999`, `:1003`, `:1120`)

O efeito depende do chamador:

| Chamador | Efeito da ausência de `cods` |
|---|---|
| `processar_cartorio` | **Não impede sempre**. Um único `out` nasce antes do laço e é reutilizado entre colaboradores; a chave é somente `data`. Se colaborador anterior registrou aquela data, a célula `nunca_bateu` passa pelo teste e chama `perguntar_pela_celula`. Se a data ainda não existe no acumulado, não chama. (`app/ponto/management/commands/processar_cartorio.py:43`, `:59`, `:65`; `app/ponto/services/cartorio.py:759`, `:768`, `:769`, `:770`, `:1120`) |
| `julgar_celula` | Cria `out` novo e passa uma única célula: no ramo sem adesão, `_julgadas` permanece vazio e a fábrica não é chamada. (`app/ponto/services/cartorio.py:1559`, `:1560`, `:759`, `:769`) |
| `rejulgar_furos_do_vinculo` | Cria `out` novo por invocação: mesma obstrução, caso entre no ramo sem adesão. (`app/escala/regua_defesa.py:691`, `:692`; `app/ponto/services/cartorio.py:1003`, `:759`, `:769`) |
| `celula_veredito_velho` | Passa `contadores()` novo para cada colaborador: mesma obstrução. (`app/ponto/management/commands/celula_veredito_velho.py:139`, `:144`, `:145`) |
| `bin/rejulgar_o118_col221.py` | Usa `out` novo e apenas um colaborador: mesma obstrução se esse ramo for alcançado com `APLICAR=True`. (`bin/rejulgar_o118_col221.py:20`, `:23`, `:24`, `:25`) |

`contadores()` não inicializa `cods`; ela nasce sob demanda no ramo normal. (`app/ponto/services/cartorio.py:1477`, `:1478`, `:1490`, `:1120`)

`cluster_espurio` **não recebe nem interpreta os valores dos códigos**: o wrapper usa apenas a existência de qualquer entrada em `_julgadas` para chamar `_cluster(c, hoje)`. Assim, um acumulado anterior também permite essa chamada para colaborador que não registrou códigos próprios. O serviço lê células/ATA do colaborador atual. (`app/ponto/services/cartorio.py:759`, `:763`, `:765`, `:767`; `app/ponto/services/cluster_espurio.py:22`, `:23`, `:26`, `:27`, `:77`)

A releitura pós-emissão **não usa `cods` como gatilho**: dispara pelo aumento de `emitidos` na chamada corrente, cria `_conv` novo, executa novamente o corpo e mescla os códigos encontrados em `out['cods']`. Não há nova chamada à fábrica após essa releitura. No ramo sem adesão, não ocorre emissão no corpo e esse delta não nasce. (`app/ponto/services/cartorio.py:743`, `:798`, `:810`, `:811`, `:812`, `:823`, `:1003`)

Passar pelo teste de data apenas **alcança a fábrica**; não garante pergunta nova. A fábrica ainda verifica `trabalha`, situação ativa, futuro, vencimentos, veredito e deduplicação por motivo. `nunca_bateu` pertence a `VEREDITOS_ACUSAM`, portanto esse veredito, isoladamente, não a veta. (`app/chamados/services/fabrica_da_celula.py:78`, `:80`, `:85`, `:86`, `:101`, `:106`, `:109`; `app/escala/models.py:1735`, `:1736`)

## 30/10 07:38 · sessao e3bb63ac3aef · - · - · origem=Ronald (tmux)

Referências e limites para consolidação:

- **L-099:** ESTRUTURA significa contradição/cálculo errado dado o cadastro; CADASTRO significa DNA que não descreve batidas; BATIDA significa falta/sobra de batida (`CLAUDE.md:306-316`).
- O juiz efetivo é `app/ponto/management/commands/e6_oraculo.py:53`. A decisão prioriza cadastro sem folga/vínculo (`:90-91`), batida solta declarada pelo realizado (`:96-97`), paridade do conjunto atribuído ao dia (`:98-99`) e presença na lista do MOTOR `dias_cadastro_x_realidade` (`:100`). Os insumos são montados em `:118-155`; batida solta real é conferida em `:158-172`.
- **A sonda não fornece os insumos completos desse juiz:** sua lista de batidas inclui dias vizinhos, sem atribuição do oráculo ao dia; também faltam `dias_cadastro_x_realidade`, resultado do realizado e flags de cadastro. Portanto dono E6 conclusivo por célula é **[não verificado]**.
- Observação permitida pelo dado: nas quatro células de col451, DNA cobra entrada às 19h em dias ímpares e a janela mostra entradas próximas de 19h nos dias pares; na 140114, a resposta/batida 17:30 difere do marco 21:30 e as saídas mostradas próximas de 06:30 diferem do marco05:42. Atribuir isso definitivamente a CADASTRO, excluindo BATIDA, não está provado **[não verificado]**.
- **Dono da contradição “fio morto” é ESTRUTURA**, expressamente documentado em `app/chamados/services/vigia_fio.py:9-14`; isso não identifica automaticamente o dono do furo ou do descompasso de horário. O próprio vigia declara que não deriva o dono L-099 em `:99-101`.

**P50170: cláusula exata não demonstrável.**

`pergunta_viva` delega ao cálculo em `app/chamados/juizes.py:795`. Para P50170, sem validação, sem resposta, via fantasma, materializada e célula `furo`, os únicos retornos `False` anteriores ao ramo de célula acusando compatíveis com os campos fornecidos são:

1. Competência trancada por leitura, `app/chamados/juizes.py:1010-1011`, cujo julgamento exige tranca e ausência de toque humano posterior (`:875-907`).
2. Marco próprio comprovado aceso na **ATA**, apesar de outro marco acusar, `app/chamados/juizes.py:1050-1052`. `orfao_14h` cobra `S/hf` (`:823-828`), e a prova lê `cel.ata['lampadas'][...]['acesa']` (`:866-872`).

A sonda não inclui tranca/toques nem ATA/`acesa`; o bloco `dna.lampadas` é previsão e não satisfaz essa leitura (`app/chamados/juizes.py:849`, `:866-872`). Assim, escolher entre essas cláusulas é **[não verificado]**.

`fantasma` não pertence a `_ENCERRA_POR_IDENTIDADE` (`app/chamados/juizes.py:1039-1045`). Se nenhum dos dois degraus acima a encerrou, célula `furo` produz `True` imediatamente (`:1054-1057`); sequer chega ao teste final de materialização (`:1082-1083`). A disputa fechada não é consultada como causa de morte nesse corpo (`:1005-1087`).

## 30/10 07:33 · sessao b270d6c7c46f · O261-N · sonda · origem=Code

**A sonda não comprova a premissa de “chamado vivo e nenhuma pergunta”:** o CH#35679 aparece como `resolvido`, a célula apresenta `np=4` e o colaborador tem 12 perguntas; o significado de `np` e a vitalidade dessas perguntas não foram detalhados. [nao verificado] `resolvido` está fora de `VIVOS`, e o censo seleciona somente chamados em `VIVOS`. (`app/chamados/catalogo/motor.py:24–31`; `app/chamados/services/vigia_fio.py:186`.)

1. **Batida e ramo do julgamento.** Não tem batida não retratada segundo o anexo: `vida=0`, `nao_retratadas=0`, `ADESAO=None`. [nao verificado] O código procura a primeira batida da vida com `retratada_em IS NULL` e deriva dela a adesão; sem essa batida, entra em **`if adesao is None`**, não no ramo normal. (`app/ponto/services/cartorio.py:899–907`.)

   Esse ramo lavra `nunca_bateu` sem cobertura integral, carimba a célula e retorna antes do ramo normal. (`app/ponto/services/cartorio.py:971–1004`.) Portanto, esse é o ramo determinado pelos insumos fornecidos; a execução histórica não tem trilha anexada. [nao verificado]

2. **Emissor do chamado e autorização pela regra.** O módulo informado para CH#35679 é `batida_ausente`; seu emissor histórico exato não consta da sonda. [nao verificado] O emissor usado pelo cartório é `detectar_ausencias.Command`, cujo `_abrir_chamado` abre esse módulo em `app/ponto/management/commands/detectar_ausencias.py:688–697`; a ligação do cartório está em `app/ponto/services/cartorio.py:1535` e `:1560–1561`.

   Não se pode atribuir CH#35679 exclusivamente a esse método: existem outros escritores com o mesmo módulo/categoria/subcategoria em `app/ponto/management/commands/reconciliar_grade.py:499–510` e `app/chamados/reconciliador.py:862–872`.

   **Nesse ramo, a regra escrita manda não emitir cobrança de furo:** `HX-CARTORIO-NUNCA-BATEU` declara “Dívida de ADESÃO” e “carimba (registro) e não emite”. (`app/ponto/services/cartorio.py:902–906`.) Além disso, `nunca_bateu` pertence a `VEREDITOS_QUE_VETAM`; o gate devolve `NUNCA_BATEU`, e o emissor retorna antes de abrir. (`app/ponto/marcos.py:119`, `:155–162`; `app/ponto/management/commands/detectar_ausencias.py:625–628`.) O veredito vigente no instante da criação e o caminho efetivamente executado não vieram no anexo. [nao verificado]

3. **Pergunta prevista e impedimento.** A regra da fábrica **admite pergunta para `nunca_bateu`**: exige colaborador ativo, célula de trabalho, data não futura e veredito acusador; `nunca_bateu` integra `VEREDITOS_ACUSAM`. (`app/chamados/services/fabrica_da_celula.py:78–87`; `app/escala/models.py:1735–1736`.) Os motivos concretos dependem da ata, dos marcos vencidos e dos motivos já perguntados. (`app/chamados/services/fabrica_da_celula.py:34–65`, `:101–109`.)

   O responsável seria `perguntar_pela_celula → perguntar → criar_ou_atualizar_disputa / gerar_perguntas_disputa`, com abertura efetiva em `PerguntaDisputa.abrir`. (`app/chamados/services/fabrica_da_celula.py:118–124`, `:143–149`; `app/chamados/services/disputa_emissao.py:389–397`.)

   **A falha de encaminhamento no ramo sem adesão:** ele carimba e retorna sem registrar a data em `out['cods']`; o chamador só chama a fábrica para datas desse mapa. O registro da data ocorre no ramo normal. (`app/ponto/services/cartorio.py:947–1003`, `:759–770`, `:1120`.) Com `out` novo, como em `julgar_celula`, esse ramo não aciona a fábrica. (`app/ponto/services/cartorio.py:1559–1561`.) No lote, porém, `out` é compartilhado entre colaboradores, portanto não cabe afirmar bloqueio absoluto sem conhecer o mapa acumulado. (`app/ponto/management/commands/processar_cartorio.py:43`, `:59–65`.)

   A sonda não traz ata, marcos, motivos, vias ou estados das perguntas; **não permite concluir que falta criar uma pergunta adicional nem identificar o impedimento efetivamente ocorrido nessa célula**. [nao verificado]

4. **Quantidade de colaboradores.** O anexo mostra **um colaborador ativo sem adesão, com chamado histórico `batida_ausente` resolvido**. Não fornece o total da frota nem comprova um chamado atualmente vivo nessa condição. [nao verificado]

   Falta medir os colaboradores ativos distintos sem nenhuma batida não retratada na vida e, dentro desse conjunto, aqueles com chamado de furo em `VIVOS` apontando para célula `nunca_bateu`; para contar emissão contrária à regra no instante em que ocorreu, falta também a trilha da emissão e do veredito naquele instante. [nao verificado] Os predicados correspondentes estão em `app/ponto/services/cartorio.py:899–907`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/furo_modulos.py:15–16` e `app/ponto/marcos.py:119`.

## 30/10 07:32 · sessao 315e09821ba9 · - · - · origem=Ronald (tmux)

Confirmado: o ramo sem adesão não alimenta `out['cods']`, mas não se pode afirmar que sempre impeça a fábrica, porque o lote compartilha `out` entre colaboradores (`app/ponto/management/commands/processar_cartorio.py:43`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/management/commands/processar_cartorio.py:65`).

Com `out` novo, como em `julgar_celula`, falta a data exigida pelo wrapper e a fábrica não é chamada (`app/ponto/services/cartorio.py:1559-1562`, `app/ponto/services/cartorio.py:759`, `app/ponto/services/cartorio.py:768-770`). No lote, uma data acumulada pode satisfazer esse teste; a sonda não informa o conteúdo de `out` nem o chamador histórico. [não verificado]

## 30/10 07:32 · sessao 3144527ec7f4 · O261-N · leitura · origem=Code

1. **O ramo `HX-CARTORIO-NB-IDEM` não escreve `out['cods']`.** As únicas chaves de `out` que ele altera são:

   | Chave | Condição |
   |---|---|
   | `pulados` | Impressão igual, célula já julgada, veredito preenchido e `not forcar`; incrementa e pula a célula. |
   | `nunca_bateu` | Célula que passou pela guarda anterior, inclusive sem `apply_`. | [nao verificado]
   | `carimbadas` | Depois da lavra/carimbo, com `apply_`. |

   Fontes: [app/ponto/services/cartorio.py:943](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:943), [cartorio.py:946](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:946), [cartorio.py:1002](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:1002).

   `_cods_nb` é variável local: serve para derivar o veredito; a lavra recebe **`codigos=[]`**, e o ramo termina em `return`. Portanto, isso também vale quando ele lavra o veredito derivado por cobertura integral. Não incrementa `julgadas` nem escreve outra chave usada para habilitar a fábrica. [app/ponto/services/cartorio.py:965](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:965), [cartorio.py:970](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:970), [cartorio.py:981](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:981), [cartorio.py:1003](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:1003).

2. **A condição exata no chamador é `apply_` verdadeiro e `_cel.data in _julgadas`, percorrendo `cels`.** Não exige códigos preenchidos, contador `julgadas` positivo ou determinado veredito nesse ponto. [app/ponto/services/cartorio.py:754](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:754), [cartorio.py:759](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:759), [cartorio.py:768](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:768).

   A cadeia é:

   - `out` chega como argumento de `julgar_colab`; o corpo inicial e a segunda passada de `rejulgar` recebem esse mesmo objeto. [app/ponto/services/cartorio.py:709](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:709), [cartorio.py:740](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:740), [cartorio.py:744](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:744).
   - O ramo principal calcula `cods`, incrementa `julgadas` e grava `out.setdefault('cods', {})[data] = list(cods)`, **inclusive quando a lista está vazia**. [app/ponto/services/cartorio.py:1108](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:1108), [cartorio.py:1119](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:1119).
   - Depois dessas passadas, `_julgadas = out.get('cods') or {}`. A eventual releitura O222 mescla `_conv['cods']` **depois** do laço de perguntas. [app/ponto/services/cartorio.py:759](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:759), [cartorio.py:798](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:798), [cartorio.py:812](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:812).

   **Ressalva decisiva:** `processar_cartorio` cria um único `out` antes do laço de colaboradores. Como a seleção usa somente a **data**, uma entrada deixada por colaborador anterior pode habilitar `perguntar_pela_celula` para uma célula `nunca_bateu`, mesmo sem escrita pelo ramo NB. Já `julgar_celula` cria contadores novos para sua chamada. [app/ponto/management/commands/processar_cartorio.py:43](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/management/commands/processar_cartorio.py:43), [processar_cartorio.py:59](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/management/commands/processar_cartorio.py:59), [processar_cartorio.py:65](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/management/commands/processar_cartorio.py:65), [app/ponto/services/cartorio.py:769](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:769), [cartorio.py:1559](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:1559).

3. **Sim, existem emissões fora do cartório.** O único chamador direto de produção de `perguntar_pela_celula` localizado é `julgar_colab`, sob a condição acima; signal e vigia de hora chegam por `julgar_celula`. [app/ponto/services/cartorio.py:770](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:770), [app/ponto/signals.py:93](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/signals.py:93), [app/ponto/services/vigia_de_hora.py:130](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/vigia_de_hora.py:130).

   - **Perguntas pelo lote:** o cron configurado `disparar_perguntas_competencia --comp-corrente --apply` chama diretamente `fabrica_da_celula.perguntar`. Seu caminho de células exige `trabalha=True`, colaborador ativo, motivos pela ata/teto temporal, veredito vazio ou em `VEREDITOS_ACUSAM` e ausência de pergunta não duplicata do fato. **`nunca_bateu` pertence à tupla aceita.** [app/config/crons.py:1456](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/config/crons.py:1456), [app/chamados/management/commands/disparar_perguntas_competencia.py:76](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/management/commands/disparar_perguntas_competencia.py:76), [disparar_perguntas_competencia.py:105](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/management/commands/disparar_perguntas_competencia.py:105), [disparar_perguntas_competencia.py:111](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/management/commands/disparar_perguntas_competencia.py:111), [disparar_perguntas_competencia.py:120](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/management/commands/disparar_perguntas_competencia.py:120), [disparar_perguntas_competencia.py:124](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/management/commands/disparar_perguntas_competencia.py:124), [disparar_perguntas_competencia.py:288](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/management/commands/disparar_perguntas_competencia.py:288), [app/escala/models.py:1735](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/escala/models.py:1735).
   - **Fio mudo no mesmo lote:** também seleciona chamado vivo ligado à célula, sem pergunta viva, com motivo pela ata, colaborador ativo e sem pergunta existente do mesmo fato; não exclui NB explicitamente. [app/chamados/management/commands/disparar_perguntas_competencia.py:155](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/management/commands/disparar_perguntas_competencia.py:155), [disparar_perguntas_competencia.py:163](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/management/commands/disparar_perguntas_competencia.py:163), [disparar_perguntas_competencia.py:167](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/management/commands/disparar_perguntas_competencia.py:167), [disparar_perguntas_competencia.py:178](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/management/commands/disparar_perguntas_competencia.py:178), [disparar_perguntas_competencia.py:220](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/management/commands/disparar_perguntas_competencia.py:220).
   - **Chamado de adesão:** `apurar_furos_diarios → sincronizar_adesao` usa células `nunca_bateu` da janela corrente e colaboradores ativos; exclui quem tem batida não retratada, isenção ou cobertura atual diferente de falta. Abre chamado `sem_previsao_escala`, subcategoria `nunca_bateu`, por colaborador/período. [app/config/crons.py:1444](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/config/crons.py:1444), [app/escala/management/commands/apurar_furos_diarios.py:17](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/escala/management/commands/apurar_furos_diarios.py:17), [app/chamados/services/adesao.py:32](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/adesao.py:32), [adesao.py:64](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/adesao.py:64), [adesao.py:68](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/adesao.py:68), [adesao.py:74](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/adesao.py:74).

   O reconciliador c2 e o emissor de ausências, por sua vez, consultam `celula_veta_cobranca`, que veta explicitamente `nunca_bateu`. [app/chamados/reconciliador.py:838](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/reconciliador.py:838), [app/ponto/management/commands/detectar_ausencias.py:625](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/management/commands/detectar_ausencias.py:625), [app/ponto/marcos.py:119](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/marcos.py:119), [marcos.py:161](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/marcos.py:161).

4. **A fábrica não recusa especificamente `nunca_bateu` nem códigos vazios.** Ela não consulta `out['cods']` ou `cel.protestos`; seu filtro de veredito aceita NB por pertencer a `VEREDITOS_ACUSAM`. Um veredito derivado fora dessa tupla é barrado. [app/chamados/services/fabrica_da_celula.py:68](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:68), [fabrica_da_celula.py:86](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:86), [app/escala/models.py:1735](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/escala/models.py:1735).

   As guardas efetivas são:

   - Célula trabalhando, colaborador ativo e data não futura. [app/chamados/services/fabrica_da_celula.py:78](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:78).
   - Marcos vencidos pelo instante; sem marcos, somente o dia passado segue pelo caminho da lei das batidas. [app/chamados/services/fabrica_da_celula.py:27](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:27), [fabrica_da_celula.py:45](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:45).
   - Ata permitindo perguntar, marco faltante vencido e motivo reconhecido; sem ata comparável ao instante, retorna vazio. [app/chamados/services/fabrica_da_celula.py:49](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:49), [fabrica_da_celula.py:59](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:59).
   - Deduplicação por motivo já perguntado no dia, excluindo `VIA_DUPLICATA`; só chama o escritor se `ms` não estiver vazio, com push silenciado. [app/chamados/services/fabrica_da_celula.py:101](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:101), [fabrica_da_celula.py:109](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:109), [fabrica_da_celula.py:147](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/chamados/services/fabrica_da_celula.py:147).

5. **Efeito de acrescentar somente `out['cods'][data] = []`:**

   | Consumidor | O que muda |
   |---|---|
   | **Cluster espúrio** | O mapa passa a ser verdadeiro se estava vazio, habilitando `_cluster(c, hoje)`. Não recebe as datas/códigos do mapa; mantém seus próprios filtros e assinatura de órfãs/lâmpadas. [app/ponto/services/cartorio.py:763](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:763), [app/ponto/services/cluster_espurio.py:22](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cluster_espurio.py:22), [cluster_espurio.py:73](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cluster_espurio.py:73). |
   | **O222: releitura pós-emissão** | A inclusão não dispara a releitura: o gatilho é crescimento de `emitidos` desde `_e0`. A releitura mescla os códigos de `_conv`, soma suas `julgadas` e corrige `protestos` pelo delta das células. [app/ponto/services/cartorio.py:743](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:743), [cartorio.py:798](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:798), [cartorio.py:809](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:809). |
   | **Contadores** | Não são derivados do tamanho do mapa. Só acrescentar a data não incrementa `julgadas`, `protestos`, `nunca_bateu` ou `carimbadas`; os incrementos são explícitos. `perguntas_no_julgamento` pode crescer se a fábrica retornar motivos. [app/ponto/services/cartorio.py:946](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:946), [cartorio.py:1002](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:1002), [cartorio.py:1119](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:1119), [cartorio.py:1292](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:1292), [cartorio.py:771](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/cartorio.py:771). |
   | **Passivo `furos_sem_cobranca`** | Não lê esse mapa: seleciona células persistidas com `veredito='furo'` e protestos cobráveis. Preservar NB e `codigos=[]` mantém a célula fora desse universo. [app/ponto/services/furos_sem_cobranca.py:35](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/furos_sem_cobranca.py:35), [furos_sem_cobranca.py:55](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/furos_sem_cobranca.py:55), [furos_sem_cobranca.py:61](/home/ronald/codex_ws/4f8f7e5add842cd053ead0b053c86bb5aa2d1ad8/app/ponto/services/furos_sem_cobranca.py:61). |

## 30/10 07:31 · sessao 315e09821ba9 · - · - · origem=Ronald (tmux)

A pergunta prevista nasce de `perguntar_pela_celula`: calcula motivos e chama `perguntar` quando há motivos faltantes (`app/chamados/services/fabrica_da_celula.py:143-149`); esta abre/agrega disputa e gera perguntas (`app/chamados/services/fabrica_da_celula.py:118-124`), com abertura efetiva em `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:389-397`).

A regra executável admite colaborador ativo, célula de trabalho, dia não futuro e veredito acusador (`app/chamados/services/fabrica_da_celula.py:78-87`); `nunca_bateu` integra `VEREDITOS_ACUSAM` (`app/escala/models.py:1735-1736`). Os motivos concretos dependem dos marcos faltantes da ata e do vencimento por instante (`app/chamados/services/fabrica_da_celula.py:34-65`). Portanto, `nunca_bateu` não é veto executável à pergunta nesses caminhos.

O impedimento estrutural no julgamento é verificável: `julgar_colab` chama a fábrica somente para datas presentes em `out['cods']` (`app/ponto/services/cartorio.py:759`, `app/ponto/services/cartorio.py:768-770`). O ramo `adesao is None` lavra/carimba e retorna sem registrar essa data em `out['cods']` (`app/ponto/services/cartorio.py:947-1003`); o registro ocorre no ramo normal (`app/ponto/services/cartorio.py:1120`). Assim, pelo ramo sem adesão, essa fábrica não é chamada no julgamento.

Há outra limitação: a fábrica deduplica pelos motivos já existentes no dia, excluindo apenas `VIA_DUPLICATA` (`app/chamados/services/fabrica_da_celula.py:101-109`). A sonda informa `np=4` na célula e 12 perguntas no colaborador, mas não fornece motivos, vias, estados, disputa nem vitalidade; não permite afirmar “nenhuma pergunta”, obrigação de pergunta adicional ou qual deduplicação incidiu no caso. [não verificado]

O comentário do lote que chama `nunca_bateu` de resolução do RH (`app/chamados/management/commands/disparar_perguntas_competencia.py:79-85`) conflita com o filtro executável atual, que lê `VEREDITOS_ACUSAM` (`app/chamados/management/commands/disparar_perguntas_competencia.py:86`, `app/chamados/management/commands/disparar_perguntas_competencia.py:120-123`).

## 30/10 07:31 · sessao c31395441520 · - · - · origem=Ronald (tmux)

- **O anexo não demonstra chamado vivo na célula 143031:** informa CH#35679 `status=resolvido` [não verificado: dado anexado, sem arquivo:linha]. `resolvido` está em `ENCERRADOS`, fora de `VIVOS` (`app/chamados/catalogo/motor.py:24`, `:30`). O censo só seleciona `status_local__in=VIVOS` (`app/chamados/services/vigia_fio.py:186`).

- **Emissor canônico do módulo:** `batida_ausente` é definido em `app/ponto/management/commands/detectar_ausencias.py:29`; `_abrir_chamado` começa em `:570` e abre o chamado em `:688`, com categoria/subcategoria em `:696-697`. **Não é possível identificar esse método como emissor histórico de CH#35679 somente pela sonda** [não verificado]: há outros escritores do mesmo módulo e categoria/subcategoria em `app/ponto/management/commands/reconciliar_grade.py:499` (módulo `CANON`, definido em `:54`) e `app/chamados/reconciliador.py:862`.

- **Pela regra atual, com `veredito=nunca_bateu`, a emissão é vetada:** o conjunto `VEREDITOS_QUE_VETAM` contém `nunca_bateu` (`app/ponto/marcos.py:119`), e o gate devolve `NUNCA_BATEU` (`:161-162`). O emissor canônico consulta esse gate e retorna antes de abrir (`app/ponto/management/commands/detectar_ausencias.py:625-628`); a grade também consulta e retorna (`app/ponto/management/commands/reconciliar_grade.py:493-496`); c2 também consulta e retorna (`app/chamados/reconciliador.py:838-842`).

- **Ramo sem adesão:** o comentário `HX-CARTORIO-NUNCA-BATEU` diz “Divida de ADESAO, nao de batida” e “carimba (registro) e nao emite” (`app/ponto/services/cartorio.py:902-906`). O ramo `if adesao is None` está em `:907` e termina com `return` em `:1003`, antes das chamadas do emissor no ramo normal (`:1358`, `:1394`).

## 30/10 07:30 · sessao e2aa7d0d8d1d · - · - · origem=Ronald (tmux)

Chamador direto de produção encontrado: somente `julgar_colab` → `perguntar_pela_celula`, em `app/ponto/services/cartorio.py:758` e `app/ponto/services/cartorio.py:770`. Signal e vigia chegam indiretamente pelo cartório: `app/ponto/signals.py:85`, `app/ponto/signals.py:93`, `app/ponto/services/vigia_de_hora.py:130`.

Existem caminhos externos que impedem concluir “NB nunca recebe pergunta”:

- **Lote automático:** `disparar_perguntas_competencia`, encadeado com `--comp-corrente --apply`, chama diretamente `fabrica_da_celula.perguntar`; não chama `perguntar_pela_celula`. Fontes: `app/config/crons.py:1456`, `app/chamados/management/commands/disparar_perguntas_competencia.py:277`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`.
- **NB passa pelo filtro do lote:** exige célula trabalhando, colaborador ativo, motivos pela ata/teto temporal, veredito vazio ou integrante de `VEREDITOS_ACUSAM` e fato ainda sem pergunta não duplicata. `nunca_bateu` integra explicitamente essa tupla. Fontes: `app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/chamados/management/commands/disparar_perguntas_competencia.py:105`, `app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `app/chamados/management/commands/disparar_perguntas_competencia.py:120`, `app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/escala/models.py:1735`.
- **O segundo universo do lote é fio mudo:** chamado vivo ligado à célula, sem pergunta viva, motivo pela ata, colaborador ativo, sem pergunta existente do mesmo fato; não contém exclusão explícita de NB. Fontes: `app/chamados/management/commands/disparar_perguntas_competencia.py:155`, `app/chamados/management/commands/disparar_perguntas_competencia.py:163`, `app/chamados/management/commands/disparar_perguntas_competencia.py:167`, `app/chamados/management/commands/disparar_perguntas_competencia.py:178`, `app/chamados/management/commands/disparar_perguntas_competencia.py:220`.
- **Chamado de adesão:** `apurar_furos_diarios` → `sincronizar_adesao`; universo de células NB na janela atual, colaborador ativo; exclui batida não retratada, isenção e ausência cobrindo hoje de tipo diferente de falta; abre pai por colaborador/período com módulo `sem_previsao_escala`, subcategoria `nunca_bateu`. Fontes: `app/config/crons.py:1444`, `app/escala/management/commands/apurar_furos_diarios.py:17`, `app/chamados/services/adesao.py:32`, `app/chamados/services/adesao.py:64`, `app/chamados/services/adesao.py:68`, `app/chamados/services/adesao.py:74`.
- **Reconciliador c2 veta chamado diário NB:** consulta `celula_veta_cobranca` e retorna; NB está na tupla de vetos. Fontes: `app/chamados/reconciliador.py:838`, `app/chamados/reconciliador.py:842`, `app/ponto/marcos.py:119`, `app/ponto/marcos.py:161`.
- **Detector de ausências também veta NB:** antes da emissão/fio, em `app/ponto/management/commands/detectar_ausencias.py:625`.
- **Crons de turno não resgatam NB real sem batidas:** `processar_alertas_turno` exige turno aberto e entrada; `processar_alertas_avancados` exige turno aberto cuja entrada extrai. Podem potencialmente tratar NB desatualizado que já possui entrada; ocorrência efetiva não foi verificada. Fontes: `app/ponto/management/commands/processar_alertas_turno.py:88`, `app/ponto/management/commands/processar_alertas_turno.py:92`, `app/ponto/management/commands/processar_alertas_turno.py:384`, `app/ponto/management/commands/processar_alertas_avancados.py:63`, `app/ponto/management/commands/processar_alertas_avancados.py:66`, `app/ponto/management/commands/processar_alertas_avancados.py:158`.
- **Passivo/manual com chamado existente:** `perguntar_chamados_sem_pergunta --apply` e `cobrar_dia` passam a causa `fabrica_nao_rodou` ao agrupador de perguntas. O caminho pela ata não exclui NB explicitamente; as guardas do escritor são ausência e deduplicação. Fontes: `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:20`, `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:26`, `app/chamados/services/pergunta_no_app.py:153`, `app/chamados/services/pergunta_no_app.py:159`, `app/chamados/services/cobrar_dia.py:40`, `app/chamados/services/cobrar_dia.py:44`, `app/chamados/services/disputa_emissao.py:693`, `app/chamados/services/disputa_emissao.py:969`, `app/chamados/services/disputa_emissao.py:978`, `app/chamados/services/disputa_emissao.py:763`.

## 30/10 07:29 · sessao 347849f0e4f1 · - · - · origem=Ronald (tmux)

- **Cluster espúrio:** usa `out['cods']` indiretamente por `_julgadas = out.get('cods') or {}` e testa apenas se o mapa é não vazio; então chama `_cluster(c, hoje)`, sem passar suas datas ou códigos. Adicionar `{data: []}` habilita essa chamada se antes o mapa estava vazio. A reconciliação continua exigindo colaborador ativo e escala ativa sem fim; a assinatura continua exigindo célula de trabalho, pelo menos duas órfãs, lâmpada apagada e rajada de até 15 minutos. (`app/ponto/services/cartorio.py:759`, `app/ponto/services/cartorio.py:763`, `app/ponto/services/cartorio.py:767`, `app/ponto/services/cluster_espurio.py:22`, `app/ponto/services/cluster_espurio.py:24`, `app/ponto/services/cluster_espurio.py:28`, `app/ponto/services/cluster_espurio.py:30`, `app/ponto/services/cluster_espurio.py:73`, `app/ponto/services/cluster_espurio.py:75`.)

- **O222 / releitura depois da emissão:** o gatilho não consulta `cods`: compara `out.get('emitidos', 0) > _e0`, sendo `_e0` capturado antes do corpo. Quando dispara, cria `_conv`, executa novamente o corpo com toda a lista `cels`, mescla `_conv.get('cods')` em `out['cods']`, soma `_conv['julgadas']` a `relidas_pos_emissao` e corrige `protestos` pelo delta dos objetos. Portanto, adicionar uma data a `cods` não dispara diretamente a releitura. (`app/ponto/services/cartorio.py:743`, `app/ponto/services/cartorio.py:798`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`, `app/ponto/services/cartorio.py:811`, `app/ponto/services/cartorio.py:812`, `app/ponto/services/cartorio.py:813`, `app/ponto/services/cartorio.py:814`.)

- **Contadores:** não derivam seus valores de `len(out['cods'])`. O ramo principal incrementa `julgadas` explicitamente e depois escreve o mapa; `protestos` depende da variável local `cods`; o ramo NB incrementa `nunca_bateu`, além de `carimbadas` quando aplica, ou `pulados` na guarda de idempotência. Somente inserir `{data: []}` não altera esses incrementos. `perguntas_no_julgamento` pode crescer indiretamente se a fábrica retornar motivos, pela quantidade retornada. (`app/ponto/services/cartorio.py:1119`, `app/ponto/services/cartorio.py:1120`, `app/ponto/services/cartorio.py:1292`, `app/ponto/services/cartorio.py:1293`, `app/ponto/services/cartorio.py:943`, `app/ponto/services/cartorio.py:944`, `app/ponto/services/cartorio.py:946`, `app/ponto/services/cartorio.py:1002`, `app/ponto/services/cartorio.py:771`, `app/ponto/services/cartorio.py:772`.)

- **Passivo `furos_sem_cobranca`:** não lê esse mapa; seleciona células persistidas por `veredito='furo'` e pelos protestos cobrados. Inserir somente uma data em `out['cods']`, preservando o veredito NB e `codigos=[]`, não inclui a célula nesse universo. (`app/ponto/services/furos_sem_cobranca.py:35`, `app/ponto/services/furos_sem_cobranca.py:55`, `app/ponto/services/furos_sem_cobranca.py:61`, `app/ponto/services/cartorio.py:980`, `app/ponto/services/cartorio.py:981`.)

- **Ressalva do lote:** `out` é criado uma única vez antes do laço de colaboradores, e cada chamada recebe o mesmo objeto. Como a fábrica testa somente a presença da **data**, uma entrada herdada de colaborador anterior já pode habilitar fábrica e cluster para um colaborador NB, mesmo sem escrita de `cods` pelo ramo NB. (`app/ponto/management/commands/processar_cartorio.py:43`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:759`, `app/ponto/services/cartorio.py:763`, `app/ponto/services/cartorio.py:769`.)

- A referência em `celula_veredito_velho` a `derivar_veredito(out['cods'][data])` descreve a implementação antiga; o código atual intercepta a lavra e chama `julgar_colab(..., out=contadores(), forcar=True)`. (`app/ponto/management/commands/celula_veredito_velho.py:12`, `app/ponto/management/commands/celula_veredito_velho.py:19`, `app/ponto/management/commands/celula_veredito_velho.py:130`, `app/ponto/management/commands/celula_veredito_velho.py:144`.)

## 30/10 05:47 · sessao fa471d9a91b1 · O261-FIO6 · sonda · origem=Code

**As seis são ESTRUTURA quanto à contradição do fio:** essa classificação é explícita no próprio vigia e corresponde à L-099. Isso não determina o dono de eventual divergência entre DNA e batidas. (`app/chamados/services/vigia_fio.py:9`; `CLAUDE.md:306`)

**A sonda permite identificar a morte por validação em quatro células, mas não permite atribuir um escritor causal específico às seis.** Faltam ata, DNA, contexto dos chamados e trilha dos carimbos. [nao verificado]

Os IDs e estados de produção abaixo vêm exclusivamente do anexo, sem arquivo local para citação. [nao verificado]

| Célula / dia / chamado | Por que não há fio vivo; qual pergunta deveria viver | Dono L-099 | Chamado deveria continuar vivo? |
|---|---|---|---|
| **143031 / col1005 / 29-10 / #35679** [nao verificado] | Nenhuma pergunta apresentada. Motivo que deveria ser emitido e bloqueio efetivamente percorrido: **[nao verificado]**. Há uma lacuna demonstrável no caminho `nunca_bateu`: o corpo lavra a célula e retorna sem registrar a data em `out['cods']`; a fábrica é chamada somente para datas presentes nesse mapa. (`app/ponto/services/cartorio.py:981`, `:1003`, `:759`, `:768`; publicação no ramo normal em `:1120`) | **ESTRUTURA do fio**. Atribuição deste caso à lacuna acima: **[nao verificado]**. | O reconciliador preserva `batida_ausente` pela regra **R** abaixo. Porém, uma nova emissão com `nunca_bateu` seria vetada: esse veredito integra `VEREDITOS_QUE_VETAM`, consumido pelo emissor. (`app/ponto/marcos.py:119`, `:161`; `app/ponto/management/commands/detectar_ausencias.py:625`) Motivo concreto de o estoque não ter sido encerrado/agrupado: **[nao verificado]**. |
| **129745 / col451 / 21-10 / #33286** [nao verificado] | **P46703 `saida_sem_entrada` e P46704 `orfao_14h` morrem por `validada_em` preenchido**, antes de consultar a célula. (`app/chamados/juizes.py:1005`) Pela regra atual, nenhuma delas deve continuar viva apenas porque o dia acusa. O emissor também retira motivos que já possuem pergunta. (`app/chamados/services/fabrica_da_celula.py:101`, `:109`) Ambas estão em `com_registro` no anexo. [nao verificado] | **ESTRUTURA do fio**, classe **V**, compartilhada por quatro células. Não há prova de erro no DNA ou falta/sobra de batida. [nao verificado] | **R** conserva o chamado. Premissa material regularizada e motivo concreto de não fechamento pelo lastro: **[nao verificado]**, conforme **L** abaixo. |
| **129747 / col451 / 23-10 / #34254** [nao verificado] | **P49422 `saida_sem_entrada` e P49423 `orfao_14h` morrem pela mesma validação prioritária.** (`app/chamados/juizes.py:1005`) **P49422 apresenta adicionalmente `sem_plantio_nenhum`**, enquanto P49423 apresenta `com_registro`. [nao verificado] A primeira classe significa que o juiz de materialização não confirmou lastro, o slot é derivável e não há plantio vivo vinculado. (`app/chamados/services/vigia_carimbo.py:106`, `:110`, `:113`) | **ESTRUTURA do fio**, classe **V**; também classe **P**, presente em uma célula. Escritor que produziu o carimbo sem lastro: **[nao verificado]**. | **R** conserva o chamado. `sem_plantio_nenhum` de P49422 não demonstra, sozinho, quais marcos #34254 cobra nem o resultado do fechador: **[nao verificado]**. |
| **129749 / col451 / 25-10 / #34850** [nao verificado] | **P50179 `saida_sem_entrada` e P50180 `orfao_14h` morrem por validação**, no primeiro teste do juiz. (`app/chamados/juizes.py:1005`) Ambas apresentam `com_registro`. [nao verificado] A existência delas impede nova emissão dos mesmos motivos pela fábrica. (`app/chamados/services/fabrica_da_celula.py:101`, `:109`) | **ESTRUTURA do fio**, classe **V**. Dono de eventual divergência DNA/batidas: **[nao verificado]**. | **R** conserva o chamado; premissa material e motivo concreto de não fechamento por **L**: **[nao verificado]**. |
| **129751 / col451 / 27-10 / #35331** [nao verificado] | **P50883 `saida_sem_entrada` e P51025 `orfao_14h` morrem por validação**, antes do veredito acusador. (`app/chamados/juizes.py:1005`) Ambas apresentam `com_registro`. [nao verificado] A fábrica exclui os motivos já existentes. (`app/chamados/services/fabrica_da_celula.py:101`, `:109`) | **ESTRUTURA do fio**, classe **V**. Dono de eventual divergência DNA/batidas: **[nao verificado]**. | **R** conserva o chamado; premissa material e motivo concreto de não fechamento por **L**: **[nao verificado]**. |
| **140114 / col129 / 25-10 / #35101** [nao verificado] | **P50169 `saida_sem_entrada` morre por validação.** (`app/chamados/juizes.py:1005`) Para **P50170 `orfao_14h`**, sem validação nem resposta no anexo, a cláusula exata que devolveu `False` é **[nao verificado]**: faltam a leitura da tranca e a lâmpada do marco, consultadas antes do veredito acusador. (`app/chamados/juizes.py:1010`, `:1050`, `:1054`) `via=fantasma` não integra as vias de identidade que encerram incondicionalmente. (`app/chamados/juizes.py:1039`) | **ESTRUTURA do fio**, classe **M**, uma célula. Não há prova de que P50170 deveria estar viva. [nao verificado] | O chamado vivo **#35101** é conservado por **R**. O fechamento de #35056 e o destino #3227 de P50169 não demonstram a regularização dos marcos de #35101. [nao verificado] Resultado concreto de **L**: **[nao verificado]**. |

As classes demonstráveis e seus escritores são:

- **V — perguntas encerradas por validação: quatro das seis células integralmente nessa classe**, as quatro de col451; P50169 também morre pelo mesmo predicado. [nao verificado] O C6 encerra imediatamente por `validada_em`; o emissor chama o fato existente de **“fio decidido”**, sem pedir nova pergunta. (`app/chamados/juizes.py:1005`; `app/chamados/management/commands/disparar_perguntas_competencia.py:210`) **Não há um escritor único demonstrado para esses registros**: existem atribuições positivas em validação, materialização e resolução por chamado. (`app/chamados/services/validacao.py:127`; `app/chamados/services/materializacao.py:514`, `:1194`; `app/chamados/services/disputa_emissao.py:1574`) Qual delas escreveu cada pergunta: **[nao verificado]**.

- **P — validada sem lastro confirmado e sem plantio vivo vinculado: uma das seis**, P49422 da célula 129747. [nao verificado] A classificação é produzida pelo leitor `vigia_carimbo.classificar`, não identifica o escritor histórico. (`app/chamados/services/vigia_carimbo.py:97`, `:106`, `:116`) A devolução da validação recusada exige `materializada_em` nulo; uma pergunta já carimbada fica fora desse mecanismo. (`app/chamados/services/validacao.py:180`) Isso explica a exclusão pelo código atual, mas não prova a sequência histórica desse caso. [nao verificado]

- **N — ausência de pergunta: uma das seis**, célula 143031. [nao verificado] O caminho `nunca_bateu` escreve veredito/ata e retorna sem alimentar o mapa exigido para chamar `perguntar_pela_celula`; esse mapa é alimentado no ramo normal. (`app/ponto/services/cartorio.py:950`, `:981`, `:1003`, `:1120`, `:768`) **A lacuna é comprovada no código; a execução responsável pela ausência nesta célula é [nao verificado]**. O motivo devido depende dos marcos faltantes e vencidos, ausentes do anexo. (`app/chamados/services/fabrica_da_celula.py:49`, `:59`)

- **M — uma pergunta validada e outra fantasma sem validação: uma das seis**, célula 140114. [nao verificado] A porta que grava `fantasma` é `PerguntaDisputa.encerrar_fantasma`; o consumidor atual exige a lâmpada do próprio marco acesa antes de chamá-la. (`app/chamados/models.py:2162`; `app/chamados/reconciliador.py:1032`, `:1092`) **Não se pode atribuir o carimbo de P50170 a esse consumidor atual** sem sua trilha histórica. [nao verificado]

**R — por que o reconciliador conserva os seis `batida_ausente`.** O catálogo associa esse módulo a `FATO_FALTA_DESCOBERTA`, mas `_veredito_premissa` não coleta esse fato; o catálogo devolve `viva=True` quando o fato não foi coletado, e o laço pula a retratação. Portanto, aqui “premissa viva” é **preservação conservadora**, não comprovação de furo material. (`app/chamados/catalogo/premissa.py:58`, `:128`; `app/chamados/reconciliador.py:262`, `:301`, `:1360`) A retratação pela célula também não age enquanto seu veredito acusa. (`app/chamados/reconciliador.py:975`)

**L — quem encerraria por lastro e por que não é possível provar a recusa individual.** `fechar_cobranca_com_lastro` delega a `lastro.fechar`; este só tenta reconciliar os chamados classificados em `com_lastro`. (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:52`; `app/ponto/services/lastro.py:306`) A classificação depende dos **marcos cobrados no contexto do chamado**, de suas lâmpadas e, no recuo, das batidas; depois ainda passa pelo gate da premissa. (`app/ponto/services/lastro.py:250`, `:264`, `:291`, `:317`; `app/chamados/models.py:470`) Esses insumos e a execução do fechador não constam da sonda: dizer que cada chamado deveria fechar, ou nomear por que concretamente não fechou, é **[nao verificado]**.

## 30/10 05:35 · sessao fb2eb8b68907 · ORCAMENTO · verificador · origem=Code

**Ainda reprovo: o novo fallback permite que o TETO engula a expiração do CLEAR e sua válvula continua condicional.** `bin/hook_stop_fila1.py:763`, `bin/hook_stop_fila1.py:893`, `bin/hook_stop_fila1.py:1102`.

1. **[P1 — novo] CLEAR vencido volta a liberar pelo TETO.** Cenário: disparo ativo, `clear_parei_desde` há 31 minutos, marco inalterado, HEAD igual e `blocks=5`; a trava fica ocupada por mais de um segundo ou ocorre uma exceção no orçamento. Ambos os caminhos retornam `None`; o CLEAR é recusado pelo PAREI genérico e o contador chega a 6, liberando por `ALARME teto`. Não saem o `siga` explícito da expiração, o reset nem o `SEGUIU`: quebra v2(b/c). Antes dos 30 minutos, o mesmo fallback pode bloquear uma espera que deveria liberar pelo item 2. `bin/hook_stop_fila1.py:761`, `bin/hook_stop_fila1.py:888`, `bin/hook_stop_fila1.py:953`, `bin/hook_stop_fila1.py:1095`, `bin/hook_stop_fila1.py:1102`.

   **O selo novo aprova essa substituição:** exige `ALLOW ALARME teto`; o caso ocupado nem estabelece `clear_parei_desde`, e o caso de exceção apaga o estado ativo. Portanto, esses casos não provam a precedência de um CLEAR vencido sob falha — classe **selo/RED sem mordida** nesse requisito. `bin/tests/test_orcamento_contexto.sh:395`, `bin/tests/test_orcamento_contexto.sh:404`, `bin/tests/test_orcamento_contexto.sh:414`, `bin/tests/test_orcamento_contexto.sh:420`.

2. **[P1 — novo, dependendo do caminho antigo] Ainda existe laço sem válvula.** Cenário: CLEAR no topo, falha ou trava persistente do orçamento, HEAD estável e `ps` mostrando continuamente `manage.py test` ou `git push`, inclusive de outra sessão. O fallback chega ao caminho antigo, mas esse caminho congela `blocks` e proíbe aplicar o TETO enquanto detectar trabalho. Cada Stop pode devolver `siga: ITEM-VIVO` indefinidamente: a afirmação nova de que “o bloqueio CONTA” não vale nesse estado. `bin/hook_stop_fila1.py:389`, `bin/hook_stop_fila1.py:733`, `bin/hook_stop_fila1.py:889`, `bin/hook_stop_fila1.py:1090`, `bin/hook_stop_fila1.py:1102`.

   Os novos casos de falha persistente usam o `ps` forjado sem trabalho e não exercitam essa combinação; o caso separado de suíte só verifica `PENDENTE` com orçamento funcionando. `bin/tests/test_orcamento_contexto.sh:35`, `bin/tests/test_orcamento_contexto.sh:342`, `bin/tests/test_orcamento_contexto.sh:398`, `bin/tests/test_orcamento_contexto.sh:416`.

3. **[P1 — dependência do acumulado] I/O bloqueante impede alcançar qualquer fallback.** Cenário: `orcamento_estado.json` é um FIFO sem escritor. A abertura para leitura fica esperando enquanto o hook segura a trava do orçamento; não ocorre exceção para `_orcamento_contido` capturar e o TETO não é alcançado. Um FIFO sem leitor em `orcamento.log` também pode prender o próprio registro de `OCUPADO`/`FALHA`. A proteção de arquivo regular foi aplicada à trava, mas não a essas aberturas: quebra a contenção exigida por v2(e)/item 3. `bin/hook_stop_fila1.py:526`, `bin/hook_stop_fila1.py:753`, `bin/hook_stop_fila1.py:777`, `bin/hook_stop_fila1.py:691`, `bin/hook_stop_fila1.py:892`.

4. **[P2 — dependência do acumulado] A hora do marco pode regredir por ordem de trava.** Cenário: A captura hora `t0` para `compact` e é suspenso; B captura `t1 > t0`, obtém a trava e grava `clear`; A retoma, obtém a trava e sobrescreve o marco com `t0`. O histórico conserva o clear de `t1`, mas a base corrente volta para `t0`, antecipando o gatilho de seis horas. É um problema de **relógio antes da seção protegida / ordem de trava**, contra a medida desde o último marco do item 1(b). `bin/marco_de_clear.py:33`, `bin/marco_de_clear.py:43`, `bin/marco_de_clear.py:53`, `bin/marco_de_clear.py:57`, `bin/orcamento_contexto.py:184`, `bin/orcamento_contexto.py:196`.

5. **[P2 — dependência do acumulado] Estado gravado com trilha falha perde o evento definitivamente.** Cenário: o estado é gravável, mas `orcamento.log` é um diretório ou recusa append. `_orc_salva` confirma o estado, `_orc_trilha` engole a falha e a lista de eventos é apagada. Se o evento era `SEGUIU`, o desfecho fica concluído e não será reapendado; o ESTADO subconta porque lê os contadores exclusivamente da trilha. Quebra o item 4, “uma linha por disparo e por desfecho”. O selo cobre a falha inversa, estado não gravável. `bin/hook_stop_fila1.py:715`, `bin/hook_stop_fila1.py:723`, `bin/hook_stop_fila1.py:694`, `bin/hook_stop_fila1.py:725`, `bin/hook_stop_fila1.py:837`, `bin/orcamento_contexto.py:219`, `bin/tests/test_orcamento_contexto.sh:406`.

Nas demais áreas lidas, sem achado adicional:

- **Prova do RELATO, PAREI comum e ESPERANDO:** a prova continua antes do orçamento; o PAREI reutiliza seu juiz; a espera compartilha uma única avaliação por Stop, preservando a prova por avanço de arquivo. `bin/hook_stop_fila1.py:1027`, `bin/hook_stop_fila1.py:916`, `bin/hook_stop_fila1.py:660`, `bin/hook_stop_fila1.py:1049`.
- **Fila vazia:** há um PREPARA-CLEAR por ciclo; mantendo o topo neutro, o Stop seguinte alcança a liberação por fila vazia. `bin/hook_stop_fila1.py:848`, `bin/hook_stop_fila1.py:876`, `bin/hook_stop_fila1.py:1063`.
- **Segundo juiz, segundo escritor e derivação paralela:** no recorte lido, a decisão usa o avaliador importado, o marco tem seu escritor próprio e o gerador do ESTADO chama os leitores compartilhados. `bin/hook_stop_fila1.py:772`, `bin/hook_stop_fila1.py:820`, `bin/marco_de_clear.py:57`, `bin/gerar_estado.py:274`.
- **Falha aberta externa:** o `except` final continua liberando; ele não resolve as esperas bloqueantes do achado 3. `bin/hook_stop_fila1.py:1117`.

Execução integral dos selos, instalação dos matchers no host e ocorrência desses cenários em produção: **[nao verificado]**.

## 30/10 04:11 · sessao 438094092922 · O261-C · verificador · origem=Code

**Achei uma brecha estrutural em (b): a dispensa do veto não acompanha a decisão de renascer.** `_costura_no_vivo` é calculado antes da trava e faz o novo veto ser pulado; depois, `abrir` trava, relê o chamado e pode encontrar um encerrado, renascendo-o sem consultar a régua. (app/chamados/reconciliador.py:798–799,843–848; app/chamados/models.py:666–687,764–770,989–1021)

O encerramento intermediário pode passar pelas portas atuais: um chamado cujo marco cobrado acendeu pode fechar, enquanto o renascimento recebe outro marco apagado do mesmo dia. Portanto, as guardas de premissa não eliminam essa brecha. (app/chamados/models.py:495–496,959–960,687; app/chamados/reconciliador.py:673–677; app/ponto/services/lastro.py:174–180) Reprodução concorrente em banco: **[não verificado]**.

- **(a) Segundo juiz/vínculo/dia: nada novo.** A impressão usa exatamente a mesma composição de leitores; o emissor consulta os mesmos predicados para vínculo presente. Existe assimetria preexistente com `ec=None`, mas os caminhos normais do emissor pulam sem vínculo. (app/ponto/services/cartorio.py:1007–1014,1347–1348,1446–1448; app/ponto/services/furos_vetados.py:25–31; app/ponto/management/commands/detectar_ausencias.py:599–605)

- **(b) Encerrado já na primeira leitura: nada.** Não satisfaz `_costura_no_vivo`; recebe o veto antes de chegar a `abrir`. A brecha acima exige mudança entre as leituras. (app/chamados/reconciliador.py:798–799,843–853)

- **(c) C1: muda, sim.** O veto alcança `orfao_14h` e `saida_sem_entrada`, inclusive fora da cerca do alcance novo. Isso bloqueia cobranças C1 antes possíveis; coincide com a regra geral contra cobrança por régua suspeita e com o alcance declarado dos quatro motivos. Nada adicional identificado como regressão funcional. (app/chamados/reconciliador.py:332,770,840–851; app/escala/regua_defesa.py:5–8)

- **Selo sem mordida: nada identificado.** R2b exige veto real e ausência de emissão; R2 fornece o controle positivo. RED executado: **[não verificado]**. (app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:174–198)

- **Relógio fora da borda: nada. Escrita fora da porta: nada. Inversão de travas: nada novo.** O acréscimo consulta dia e estado gravado, sem escrita ou aquisição de trava; permanece a brecha de decisão anterior à trava descrita acima. (app/ponto/services/furos_vetados.py:19–32; app/escala/regua_defesa.py:554–562,641–644; app/escala/servico_jornada.py:38–58; app/chamados/models.py:666–670)

## 30/10 04:10 · sessao 5604ba6bee6c · - · - · origem=Ronald (tmux)

(b) Há janela de corrida: o c2 guarda `_costura_no_vivo` antes da trava e usa esse booleano para pular o veto; `abrir` só adquire a trava depois, relê o chamado e pode renascê-lo já encerrado. O renascimento não consulta o veto da régua. Referências: `app/chamados/reconciliador.py:798`, `app/chamados/reconciliador.py:843`, `app/chamados/models.py:666`, `app/chamados/models.py:685`, `app/chamados/models.py:764`, `app/chamados/models.py:989`.

O encerramento concorrente pode ser válido: um chamado com seus marcos antigos acesos fecha por `reconciliar`, enquanto o c2 pede outro marco apagado do mesmo dia. O fechamento usa os marcos próprios; o renascimento recebe os marcos novos de `_ctx_c2`. Assim, a mesma autoridade pode permitir fechar o antigo e renascer pelo novo, com o veto omitido pelo booleano anterior. Referências: `app/chamados/models.py:495`, `app/chamados/models.py:959`, `app/chamados/models.py:687`, `app/chamados/reconciliador.py:673`, `app/ponto/services/lastro.py:174`, `app/chamados/juizes.py:435`. Não reproduzi a concorrência em banco. [não verificado]

(b), execução sequencial: nada. Encerrado desde a primeira leitura não satisfaz `_costura_no_vivo` e passa pelo veto antes de `abrir`. Referências: `app/chamados/reconciliador.py:799`, `app/chamados/reconciliador.py:843`, `app/chamados/reconciliador.py:853`.

(c): o veto alcança C1, inclusive fora do alcance novo, pois a limitação pela cerca só vale para motivos fora de `MOTIVOS_C1`. Isso impede emissões C1 vetadas anteriormente possíveis, mas é coerente com a regra existente contra cobrança por régua suspeita e com o comentário novo que declara os quatro motivos; não identifiquei regressão funcional adicional. Referências: `app/chamados/reconciliador.py:332`, `app/chamados/reconciliador.py:770`, `app/chamados/reconciliador.py:840`, `app/chamados/reconciliador.py:851`, `app/escala/regua_defesa.py:5`.

## 30/10 04:09 · sessao b9b06cd6af78 · - · - · origem=Ronald (tmux)

- **Selo sem mordida: nada identificado.** R2b chama o leitor real, exige `VETO_REGUA`, verifica retorno `False` e ausência de chamado; R2 fornece controle positivo do mesmo cenário sem veto (`app/chamados/tests/test_o261_c_viva_sem_chamado_do_dia.py:174–198`). RED executado: **[não verificado]**.
- **Relógio fora da borda: nada no acréscimo.** O dia é recebido como `_d_hz`; os predicados consultados usam template e escore gravado, sem consultar hora atual (`app/chamados/reconciliador.py:845`; `app/ponto/services/furos_vetados.py:25–32`; `app/escala/regua_defesa.py:16–33`, `:554–562`, `:641–644`).
- **Escrita fora da porta: nada no acréscimo.** O carregador consulta célula/vínculo e os predicados devolvem valores; a emissão continua em `ChamadoColaborador.abrir` (`app/escala/servico_jornada.py:38–58`; `app/ponto/services/furos_vetados.py:19–32`; `app/chamados/reconciliador.py:853`).
- **Inversão da ordem de travas: nada introduzido pelo leitor.** Ele usa SELECT comum, sem adquirir trava; `abrir` mantém `atomic` e trava do colaborador (`app/escala/servico_jornada.py:46–58`; `app/chamados/models.py:666–670`).

**Atenção para consolidar com (b):** a dispensa por `_costura_no_vivo` é decidida antes da trava (`app/chamados/reconciliador.py:798–799`, `:843`). Dentro da trava, `abrir` reconsulta e pode renascer um encerrado (`app/chamados/models.py:669–687`). Uma mudança concorrente de vivo para encerrado entre essas leituras escaparia do novo veto; reprodução desse intercalamento: **[não verificado]**.

_363 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
