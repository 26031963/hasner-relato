# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 09:19** · publicado em **06/11/2026 09:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 09:19 · sessao dd224cc5cb59 · O10-TROCA-DE-PLANTAO · censo · origem=Code

HASH LIDO = 185c878317e21679038d2274c24f4d017998ee4b

Identificador da cópia recebida; correspondência com commit por metadados Git: [nao verificado]. Resultados abaixo são censo estático; execução dos comportamentos e valores financeiros: [nao verificado]. [nao verificado]

## Q1. Escritores de `CelulaDia` editada

**Não encontrei escritor humano de produção para `origem='editada'`.** A cópia declara expressamente `editada_em` e `editada_por` como campos **sem escritor**, com o comentário “a edição humana não tem porta”; outro serviço registra “escritor de produção: nenhum declarado hoje”. Isso diverge da premissa “escritor humano existente” do anexo (`app/escala/tests/test_d3_guarda_da_linha_celula.py:50`; `app/escala/tests/test_d3_guarda_da_linha_celula.py:179`; `app/escala/services/fase_12x36.py:161`).

| Caminho existente | Campos e trilha | Atomic e chamador |
|---|---|---|
| `gerar_celulas_periodo` | Cria `colaborador,data,trabalha,origem='gerada',escala_geradora,dna`; pula editadas. Não é editor humano (`app/ponto/portas/celula.py:633`; `app/ponto/portas/celula.py:638`). | A função não abre atomic próprio; o command a envolve em atomic (`app/ponto/portas/celula.py:595`; `app/ponto/management/commands/gerar_celulas.py:310`). |
| `regenerar_celulas_vinculo` | Cria gerada; reescreve somente geradas. Guarda `dna_anterior,regenerada_em,regeneracoes`; evento leva usuário, motivo e resumo posterior, sem constituir trilha de edição humana (`app/ponto/portas/celula.py:253`; `app/ponto/portas/celula.py:257`; `app/ponto/portas/celula.py:265`; `app/ponto/portas/celula.py:330`). | Atomic em `:212`; chamado por vínculo, edição de template e signals (`app/ponto/portas/celula.py:212`; `app/colaboradores/services/vinculo.py:857`; `app/escala/services/cadastro_tipo.py:348`; `app/escala/signals.py:71`). |
| Resolver: `veredito_celula` → `lavrar_veredito` | Escreve **julgamento**, não `origem/trabalha/dna`: veredito, via, horário, protestos e status; ata quando fornecida. Evento registra ator e antes/depois; despachante registra ações/erros, sem argumento de motivo livre (`app/ponto/services/veredito_celula.py:191`; `app/ponto/services/veredito_celula.py:330`; `app/ponto/services/veredito_celula.py:334`; `app/ponto/portas/celula.py:815`; `app/ponto/portas/celula.py:819`). | Atomic no despachante; save/outbox também atomic. Entrada HTTP em `veredito_celula_view` (`app/ponto/services/veredito_celula.py:204`; `app/ponto/portas/celula.py:681`; `app/ponto/views.py:3312`). |

O modelo apenas oferece as choices e os campos de auditoria; a migration apenas cria esse schema, sem implementar o ato humano (`app/escala/models.py:1690`; `app/escala/models.py:1732`; `app/escala/migrations/0030_celuladia.py:23`).

**Duas células no mesmo atomic:** o modelo permite duas linhas de colaboradores/datas distintos: sua unicidade é `(colaborador,data)`. O escritor canônico é definido por **módulo**, `ponto/portas/celula.py`, sem limite de uma linha por transação (`app/escala/models.py:1833`; `app/core/portas.py:107`).

**Mas não existe hoje a operação humana de edição para chamar duas vezes.** O Resolver aceita somente três vereditos, e sua view processa vários dias de **um colaborador**, chamando um atomic por dia; portanto esse lote não realiza o par indivisível solicitado (`app/ponto/services/veredito_celula.py:18`; `app/ponto/services/veredito_celula.py:204`; `app/ponto/views.py:3318`; `app/ponto/views.py:3339`).

A porta existente poderia comportar mais de uma linha sem virar outro **módulo escritor**; porém sua lista atual só declara gerar/lavrar/carimbar. Os selos cobram tanto ausência de escrita fora da porta quanto pertencimento a uma operação declarada. Logo, “reusar duas edições existentes, sem acrescentar capacidade” não corresponde ao código disponível (`app/ponto/portas/celula.py:66`; `app/ponto/tests/test_chokepoint_celula_gate.py:49`; `app/ponto/tests/test_chokepoint_celula_gate.py:69`).

**Escritas `editada` encontradas em testes**, sem constituir porta humana de produção: [nao verificado]

| Forma | Sítios |
|---|---|
| Atribuição e `save` | `app/escala/tests/test_celula_dia.py:73`, `:98`, `:358`; `app/colaboradores/tests/test_vinculo_regenera_celulas.py:39`; `app/ponto/tests/test_porta_celula_dia.py:247`. |
| Criação/update direto | `app/escala/tests/test_selo_foto_folgas_relavra.py:104`; `app/escala/tests/test_sem_dia_pelo_juiz.py:46`; `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:97`; `app/ponto/tests/test_fechar3_troca_escala_gate.py:134`, `:180`, `:225`; `app/ponto/tests/test_d3_impressao_cobre_insumos.py:242`. |
| Helper que recebe a origem | `app/escala/tests/test_selo_celula_alem_do_vinculo.py:58`, chamado com editada em `:103`; `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:116`, chamado em `:128`. |

## Q2. Leitores e sobrevivência à regeneração

| Leitor | O que efetivamente lê |
|---|---|
| `EC.eh_dia_trabalho` | Retorna `cel.trabalha`, sem filtrar origem; somente sem célula usa a aritmética do vínculo (`app/escala/models.py:1087`; `app/escala/models.py:1094`). |
| `EC.marcos_do_dia` | Lê `dna.marcos`, sem filtrar origem. **Marcos vazios/nulos fazem cair no template**, mesmo existindo célula (`app/escala/models.py:1124`; `app/escala/models.py:1133`). |
| `EC.pausas_do_dia` / `intervalo_do_dia` | Leem as pausas congeladas; célula existente sem marcos devolve nenhuma pausa, sem aquele fallback (`app/escala/models.py:1163`; `app/escala/models.py:1189`; `app/escala/models.py:1202`). |
| `gerar_celulas_periodo` | Editada é pulada: **sobrevive** (`app/ponto/portas/celula.py:638`). |
| `regenerar_celulas_vinculo` / reversão | Reescreve somente gerada; até o desligamento fora da vigência exige gerada. Reversão também recusa editada: **sobrevive** (`app/ponto/portas/celula.py:232`; `app/ponto/portas/celula.py:257`; `app/ponto/portas/celula.py:558`). |
| Fechamento/motor | Previsto vem de `EC.eh_dia_trabalho`; células alimentam o motor; separação trabalho/folga consulta o vínculo (`app/ponto/services/fechamento.py:722`; `app/ponto/services/fechamento.py:741`; `app/ponto/motor_calculo_v2.py:2448`). |
| Export | Grade delega a `grade_da_celula`, que lê todas as origens, mas prefere `ata.tipo_dia` e `ata.minutos_previstos` já lavrados. Rubricas do TXT vêm do fechamento gravado (`app/folha/export.py:335`; `app/escala/services/leitor_celula.py:350`; `app/escala/services/leitor_celula.py:380`; `app/folha/export.py:439`). |

**Sobreviver à regeneração não garante ata e dinheiro atualizados.** O export pode continuar lendo a ata anterior; a impressão do cartório não inclui `trabalha`, e impressão igual pode fazer pular o julgamento (`app/escala/services/leitor_celula.py:380`; `app/ponto/services/cartorio.py:456`; `app/ponto/services/cartorio.py:1358`).

## Q3. Rubricas do dia cedido e assumido

As conclusões seguintes pressupõem o cenário perguntado: cedido sem trabalho real; assumido com trabalho real e célula/marcos coerentes. Batidas, pausas, feriados, vínculos e valores de um par concreto: [nao verificado].

| Rubrica | CEDIDO: célula folga | ASSUMIDO: célula trabalho | Quem decide / fonte |
|---|---|---|---|
| **Adicional noturno** | Sem período trabalhado, não nasce AN. | Apura AN dos períodos noturnos; a passagem folga→trabalho não cria, por si, o direito, pois folga trabalhada já participa da soma. | Períodos reais e política noturna; soma `periodos + periodos_ft`; fechamento grava resultado (`app/ponto/motor_calculo_v2.py:983`; `app/ponto/motor_calculo_v2.py:321`; `app/ponto/services/fechamento.py:909`). |
| **Intrajornada** | Sem período real, não nasce indenização. | Apura supressão conforme duração e pausa; pode ser zero. | Intervalos reais, previsto e cadastro/perfil. Regras 12x36/comercial em `app/ponto/motor_calculo_v2.py:2766` e `:3226`; mínimo/cadastro em `:1824` e `:831`. Previsto consulta célula e marcos (`app/escala/utils.py:2287`; `app/escala/utils.py:2290`). |
| **HE100 por folga/DSR trabalhado** | Sem trabalho, não há período a pagar. | Célula `True` retira o período de `periodos_ft`; passa a normal e deixa de alimentar `horas_folga_trabalhada`. | **Lê célula por EC** em `app/ponto/motor_calculo_v2.py:2448`; partição em `:2468` e `:2492`. Fechamento paga a rubrica de folga a partir de FT e escala certa (`app/ponto/services/fechamento.py:925`; `app/ponto/services/fechamento.py:949`). |
| **DSR: semana/reflexo** | Sai dos dias obrigatórios; não gera falta injustificada por não trabalhar nessa data. | Entra no previsto; trabalho normal conta para a semana. Resultado depende dos demais dias/extras. | Previsto lê célula no fechamento (`app/ponto/services/fechamento.py:722`). Motor combina previsto, períodos e datas justificadas (`app/ponto/motor_calculo_v2.py:2595`; `:2600`; `:2636`); comercial refaz o DSR em `:3452`. |
| **Furo apurado** | Sai dos obrigatórios e deixa de gerar furo por essa ausência. | Com trabalho reconhecido, não é furo; sem trabalho/abono, pode ser. | Previsto + períodos + justificativas (`app/ponto/motor_calculo_v2.py:2600`; `:2610`). O fechamento aplica teto temporal antes de lavrar (`app/ponto/services/fechamento.py:1099`). |
| **Falta/desconto decidido** | **Célula não cancela Ausencia DESCONTA aprovada preexistente.** | Também não cancela essa decisão. | Outra fonte: Ausencia aprovada + catálogo. Fechamento lê minutos decididos; TXT lê datas decididas (`app/ponto/services/fechamento.py:864`; `app/folha/export.py:87`; `:138`; `:179`; `:732`). |
| **Atraso/antecipada** | Previsto zero limita a cobrança a zero. | Novo dia normal com marcos pode sofrer pontualidade pelas regras já vigentes. | Teto pelo previsto em `app/ponto/motor_calculo_v2.py:1040`; marcos do vínculo em `:2048`; comercial chama o julgamento em `:3293`; 12x36 com escala em `:3533`. |

**“Sem HE100” precisa distinguir a causa:** editar para trabalho retira o adicional **por folga**, mas não elimina automaticamente HE100 por feriado ou excedente normal. Essas decisões usam calendário/política de feriado e excedente, respectivamente (`app/ponto/motor_calculo_v2.py:934`; `:2847`; `:3392`).

**Não identifiquei rubrica-alvo que exija alterar `ponto/motor_calculo_v2.py` para consumir esse override.** Os sítios necessários já consultam célula: classificação trabalho/folga em `:2448`, partição em `:2468`, marcos em `:2290`. Cancelar desconto já decidido pertence ao fluxo de Ausencia, cuja leitura financeira é separada (`app/folha/export.py:138`; `app/folha/export.py:179`). Prova executada de que duas edições satisfazem todo o contrato O10: [nao verificado].

**S5b histórico não descreve integralmente esta cópia:** O271 posteriormente ligou pontualidade comercial; o código efetivamente chama `_julgar_pontualidade`. Portanto não cabe afirmar que o comercial atual continua isento/`NAO_DECIDE` (`app/ponto/calculador/regras.py:455`; `app/ponto/calculador/regras.py:466`; `app/ponto/motor_calculo_v2.py:3293`).

**DSR no TXT:** o export não emite evento DSR; registra que o Domínio calcula o reflexo a partir de HE/AN. Valor efetivamente pago no Domínio: [nao verificado] (`app/folha/export.py:453`).

**Motor versus calculador:** o fechamento mensal recebe rubricas do motor; desde competência 10/2026 há também `DiaPago` versão `oraculo`, alimentado pelo calculador, sem substituir esse fechamento. O calculador igualmente consulta célula e marcos (`app/ponto/services/fechamento.py:909`; `:1085`; `:1249`; `:1278`; `app/ponto/calculador/alimentacao.py:201`; `:434`).

## Q4. Resolver dia: oferta, opções e entrada da quarta

| Camada | Comportamento atual |
|---|---|
| Juiz da oferta | `pode_ultimo_recurso` + data passada + status entre `falta,irregular,aberto,sem_certidao`; permissão fica fora do predicado (`app/colaboradores/services/calendario.py:94`; `:97`; `:103`). |
| Último recurso | Exige data anterior ao início da competência atual e ausência de pergunta viva respondida naquele dia (`app/colaboradores/services/calendario.py:472`; `:738`). |
| Chamadores | Calendário carimba o resultado por dia. Porta plural monta calendários e lê esse mesmo campo; o vigia do fio usa a plural (`app/colaboradores/services/calendario.py:132`; `:135`; `:769`; `app/chamados/services/vigia_fio.py:265`). |
| Telas | Perfil e drawer fornecem permissão `regularizar_batida` e tipos do cadastro; grade inclui o popover quando permitido/oferecido (`app/colaboradores/views.py:1948`; `:1957`; `app/colaboradores/services/drawer.py:130`; `:154`; `app/templates/colaboradores/partials/_calendario_grade.html:125`). |
| POST | `/celula/veredito/` → `veredito_celula_view`; valida permissão/veredito, recebe um colaborador e uma ou várias datas (`app/ponto/urls.py:58`; `app/ponto/views.py:3315`; `:3318`; `:3328`). |

| Opção apresentada | Quem executa |
|---|---|
| **Trabalhou no horário da escala** | Simula correções, faz flips e materializa batidas faltantes; recusa se a grade não prevê trabalho. Não transforma folga em dia normal (`app/templates/ponto/partials/_veredito_dia.html:21`; `app/ponto/services/veredito_celula.py:221`; `:295`; `:301`). |
| **Não trabalhou — falta** | Cria Ausencia `falta`, aprovada (`app/templates/ponto/partials/_veredito_dia.html:22`; `app/ponto/services/veredito_celula.py:311`; `:315`). |
| **Justificado** | Cria Ausencia aprovada do tipo escolhido (`app/templates/ponto/partials/_veredito_dia.html:23`; `app/ponto/services/veredito_celula.py:315`). |
| **Cobrar este dia** | Botão adicional, com outra porta; manda julgar/emite cobrança e pode criar pergunta no app. Não é quarto veredito do despachante (`app/templates/ponto/partials/_veredito_dia.html:25`; `app/chamados/services/cobrar_dia.py:39`; `:43`; `:47`). |

**Entrada natural da quarta opção de negócio:** mesmo partial → mesma view → despachante/lista `VEREDITOS`; o contrato nomeia justamente “Trocou de plantão” nessa porta. Porém hoje faltam os dados do par/anexos na requisição, e o serviço não possui a operação de edição identificada em Q1 (`app/docs/CORTES.md:187`; `app/templates/ponto/partials/_veredito_dia.html:17`; `app/ponto/views.py:3318`; `app/ponto/services/veredito_celula.py:18`).

Há também uma questão de alcance: **`folga_trabalhada` não está entre os status que oferecem o Resolver**, e competência corrente fica fora do último recurso. A oferta atual, sozinha, não alcança todos os cenários descritos no corte (`app/colaboradores/services/calendario.py:94`; `:738`; `app/docs/CORTES.md:187`). Inclusão atual desse mesmo Resolver diretamente no chamado de batida em folga: [nao verificado].

## Q5. `troca_turno`, `troca_de_plantao` e `efeito='abona'`

### `troca_turno`

| Aspecto | Escritor/leitor e efeito |
|---|---|
| Natureza | Choice de `Justificativa.tipo`; migration `0019` altera schema/choices. API oferece e traduz o valor; template web também oferece (`app/ponto/models.py:126`; `app/ponto/migrations/0019_tipos_justificativa.py:16`; `app/api/views.py:2070`; `:2099`; `app/templates/ponto/justificativa_form.html:43`). |
| Escritores do tipo | API atualiza/cria justificativa pendente; formulário web faz equivalente (`app/api/views.py:2001`; `:2007`; `app/ponto/views.py:1625`; `:1635`). Django admin também é editável (`app/ponto/admin.py:18`). |
| Escritores da decisão | Individual no serviço; lote na view; materialização retroativa também aprova. Registram status, usuário e horário (`app/ponto/services/justificativa.py:190`; `app/ponto/views.py:1796`; `app/ponto/services/justificativa.py:368`). |
| Leitor específico do nome | SLA de **3 dias**; fila e cron consultam essa autoridade. É prazo do DP, não regra monetária (`app/ponto/catalogo/justificativas.py:24`; `:29`; `app/ponto/views.py:1726`; `app/ponto/management/commands/alertar_justificativas_sla.py:19`). |
| Efeito financeiro genérico | Justificativa não tem campo `efeito`. Qualquer aprovada entra em `datas_justificadas`, sem filtro de tipo; isso interfere em falta injustificada/abono/DSR (`app/ponto/models.py:146`; `:159`; `app/ponto/services/fechamento.py:651`; `app/ponto/motor_calculo_v2.py:2600`; `:2606`). |
| Batida e célula | Aprovação pode criar saída manual se houver entrada + saída informada; não escreve override do par. A API anuncia `requer_saida=False` (`app/ponto/services/justificativa.py:137`; `:172`; `:190`; `app/api/views.py:2070`). |

### `troca_de_plantao` / Ausencia

**Não localizei ramo runtime nem migration de dados com esse literal.** A existência e `efeito='abona'` vêm do dado raspado fornecido; quem criou esse cadastro e quando: [nao verificado]. O código aceita tipos cadastrados e decide genericamente pelo efeito (`app/ponto/services/ausencia.py:193`; `app/ponto/catalogo/ausencias.py:249`; `:293`).

| Entidade/caminho | Escritor | Leitor/efeito atual |
|---|---|---|
| `TipoAusencia` | UI entrega nome, efeito e flags à porta `gravar_tipo_ausencia`; código permanece estável (`app/ponto/views_tipos_ausencia.py:67`; `:84`; `:94`; `app/ponto/services/ausencia.py:1285`). | `efeito` pertence a esse cadastro. `Ausencia.efeito` consulta o catálogo vivo (`app/ponto/models.py:581`; `:840`; `app/ponto/catalogo/ausencias.py:293`). |
| Ausencia do tipo cadastrado | `criar_ausencia` grava tipo, datas, minutos, documento, status e autores; entradas API/admin/PWA. Edição e decisão também passam pelo serviço (`app/ponto/services/ausencia.py:341`; `:497`; `:895`; `app/api/views.py:1820`; `app/ponto/views.py:2415`; `:2536`). | Para folha só aprovada; ABONA entra em `datas_justificadas`; ausência inteira abonada pode zerar pontualidade (`app/ponto/turnos.py:2514`; `app/ponto/services/fechamento.py:684`; `app/ponto/turnos.py:2689`). |
| Pedido ainda não aprovado | Estados abertos são aguardando documento/decisão (`app/ponto/services/ausencia.py:749`). | Não cobre folha, mas ausência não rejeitada já pode suspender cobrança. Logo “virar pedido” também envolve esse leitor (`app/ponto/turnos.py:2514`; `:2516`). |
| Migrations | Seed cria tipos por `get_or_create`; lista não contém `troca_de_plantao`. Backfill posterior ajusta pendentes conforme documento (`app/ponto/migrations/0052_seed_tipoausencia.py:9`; `:33`; `app/ponto/migrations/0057_a4_backfill_pendente.py:30`). | Aplicação dessas migrations e origem específica do cadastro em produção: [nao verificado]. |

Existe auxiliar que seleciona tipos abonados para “8932”, mas **não encontrei chamador de produção de `datas_abonadas`**. Não concluo que a troca emita 8932 hoje: o montador efetivo percorre eventos do fechamento e acrescenta faltas; sua lista contém HE, folga, AN, intra e pontualidade (`app/folha/export.py:193`; `:201`; `:439`; `:722`; `:732`).

Os números **16 lançamentos, 12 aprovados, 2 rejeitados, 2 aguardando, todos SOLO/abona** são exclusivamente o anexo do pedido; conferência por arquivo de dados da cópia ou banco: [nao verificado].

## Q6. Cartas e exigência de dois anexos

| Ponto | Situação atual |
|---|---|
| Armazenamento | `Ausencia.documento`: **um `FileField`**, em `ausencias/documentos/`, opcional. `documento_referencia` aponta ao primeiro registro do intervalo; não é segunda carta (`app/ponto/models.py:777`; `:791`). |
| Entrada inicial | API recebe `FILES['documento']`; admin/PWA repassam um documento a `criar_ausencia` (`app/api/views.py:1775`; `:1822`; `app/ponto/views.py:2418`; `:2538`). |
| Entrada posterior | API e view administrativa chamam `anexar_documento`; esta **substitui** o campo único, registra ator/antes/depois e pode passar para aguardando decisão (`app/api/views.py:3130`; `app/ponto/views.py:2150`; `app/ponto/services/ausencia.py:840`; `:854`; `:857`). |
| Leitura | Download resolve `documento_referencia or a` e abre o arquivo da raiz (`app/ponto/views.py:2115`; `:2122`). |
| Exigência existente | Flag booleana `exige_documento`; estado inicial recebe `tem_documento`, também booleano. Não conta cartas nem identifica cedente/substituto (`app/ponto/models.py:583`; `app/ponto/services/ausencia.py:763`; `:775`). |
| Garantia de dois documentos | **Não existe nesses caminhos.** Há um arquivo por registro e nenhum gate do par; outra implemen

[truncado em 20000 de 24211 caracteres]

## 06/11 09:17 · sessao 501a50827797 · RE-O10-TROCA-DE-PLANTAO · censo · origem=Code

HASH LIDO = 185c878317e21679038d2274c24f4d017998ee4b [nao verificado]

| id | regra de origem (L-NNN, corte literal do aval ou arquivo:linha da regra) | entrada (model e campos, cada campo com arquivo:linha da definicao) | esperado OBSERVAVEL (o que um teste ve: retorno, linha gravada, status, texto) | mutante que tem de morder (a alteracao de codigo que este caso pega) | tipo golden/borda/adversarial | [nao verificado]
|---|---|---|---|---|---|
| G01 — cedente 12x36 | Aval A2: “cedente 12x36 e 6x1, dia cedido = folga sem desconto”; `app/docs/CORTES.md:187` | Confirmação pelo admin com par completo e duas cartas. `TipoEscala.tipo_ciclo='12x36'` (`app/escala/models.py:117`). `CelulaDia.colaborador=cedente` (`app/escala/models.py:1681`), `data=2026-11-03` (`app/escala/models.py:1685`), `trabalha=True` (`app/escala/models.py:1686`). Cartas em `Ausencia.documento` (`app/ponto/models.py:777`); ligação ao par: [nao verificado]. | Dia cedido passa a folga: `trabalha=False`; a troca não acrescenta desconto por esse dia. | Manter o dia cedido como trabalho e gerar falta pela ausência de batidas. | golden |
| G02 — cedente 6x1 | Aval A2: “cedente 12x36 e 6x1, dia cedido = folga sem desconto”; `app/docs/CORTES.md:187` | Confirmação pelo admin com par completo e duas cartas. `TipoEscala.tipo_ciclo='6x1'` (`app/escala/models.py:117`). `CelulaDia.colaborador=cedente` (`app/escala/models.py:1681`), `data=2026-11-04` (`app/escala/models.py:1685`), `trabalha=True` (`app/escala/models.py:1686`). Cartas em `Ausencia.documento` (`app/ponto/models.py:777`); ligação ao par: [nao verificado]. | Dia cedido passa a folga sem desconto, também no 6x1. | Aplicar a dispensa de desconto somente ao ciclo 12x36. | golden |
| G03 — substituto em folga | Aval A2: “substituto em folga, dia assumido = normal com AN/intra e SEM HE100”; `app/docs/CORTES.md:187` | Par completo, duas cartas e confirmação administrativa. `CelulaDia.colaborador=substituto` (`app/escala/models.py:1681`), `data=2026-11-03` (`app/escala/models.py:1685`), `trabalha=False` (`app/escala/models.py:1686`). Turno com período noturno e intervalo: `TipoEscala.hora_inicio=19:00` (`app/escala/models.py:50`), `hora_fim=07:00` (`app/escala/models.py:51`), `hora_inicio_intervalo=23:00` (`app/escala/models.py:52`), `hora_fim_intervalo=23:30` (`app/escala/models.py:53`). | Dia assumido passa a normal: `trabalha=True`; AN e intra seguem o tratamento de dia normal equivalente; nenhum HE100 decorre de a data ser originalmente folga. Quantias exatas: regra nao decide. | Preservar a classificação de folga trabalhada e pagar HE100; ou suprimir AN/intra por ser troca. | golden |
| G04 — pedido pelo substituto | `app/docs/CORTES.md:187`: “pode nascer de qualquer dos dois lados” | `Ausencia.colaborador=substituto` (`app/ponto/models.py:743`), `data=2026-11-03` (`app/ponto/models.py:750`), `documento=carta do substituto` (`app/ponto/models.py:777`). Identificação do cedente/dia cedido no pedido: [nao verificado]. Admin confirma com a carta do cedente e par completo. | A origem pelo substituto permite o mesmo ato confirmado: cedido folga sem desconto; assumido normal com AN/intra, sem HE100. | Aceitar somente pedidos iniciados pelo cedente. | golden |
| B01 — nenhum anexo | Aval A2: “troca confirmada sem os 2 anexos = recusa” | Tentativa de confirmação de par completo; `Ausencia.documento` vazio nos dois pedidos (`app/ponto/models.py:777`). | Confirmação recusada; nenhum par confirmado válido. Código de retorno e texto exato: regra nao decide. | Retirar a exigência documental da confirmação. | borda |
| B02 — falta carta do substituto | Aval A2: “troca confirmada sem os 2 anexos = recusa”; `app/docs/CORTES.md:187` | Par completo; `Ausencia.documento` preenchido para cedente e vazio para substituto (`app/ponto/models.py:777`); tentativa de confirmação administrativa. | Confirmação recusada. Uma carta não satisfaz as duas cartas exigidas. | Validar apenas a existência de algum documento. | borda |
| B03 — falta carta do cedente | Aval A2: “troca confirmada sem os 2 anexos = recusa”; `app/docs/CORTES.md:187` | Par completo; `Ausencia.documento` vazio para cedente e preenchido para substituto (`app/ponto/models.py:777`); tentativa de confirmação administrativa. | Confirmação recusada, independentemente do lado que iniciou o pedido. | Exigir apenas a carta de quem iniciou o pedido. | borda |
| B04 — somente um lado | Aval A2: “par incompleto (so um lado) = pedido, nao vale” | Um único lado de edição: `CelulaDia.colaborador=cedente` (`app/escala/models.py:1681`), `data=2026-11-03` (`app/escala/models.py:1685`), `trabalha=True` (`app/escala/models.py:1686`). Outro lado ausente, mesmo com cartas disponíveis em `Ausencia.documento` (`app/ponto/models.py:777`). | Continua sendo pedido; não produz os efeitos de troca confirmada. Nome do estado persistido: regra nao decide. | Considerar um lançamento SOLO suficiente para efetivar a troca. | borda |
| B05 — documentos sem confirmação | `app/docs/CORTES.md:187`: “o par so vale quando o admin confirma no Resolver dia com os dois documentos presentes” | Pedidos com `Ausencia.documento` preenchido (`app/ponto/models.py:777`); confirmação administrativa ainda não realizada. Campo próprio de confirmação do par: [nao verificado]. | Presença das cartas não efetiva a troca; permanece pedido sem os efeitos do ato confirmado. | Confirmar automaticamente quando o segundo documento chega. | borda |
| B06 — segunda confirmação | Aval A2: “segunda confirmacao do mesmo par = idempotente”; `CLAUDE.md:175` | Repetir duas vezes a confirmação das mesmas células: `CelulaDia.colaborador` (`app/escala/models.py:1681`) e `data` (`app/escala/models.py:1685`) idênticos nas duas chamadas. Chave formal do par: [nao verificado]. | Segunda chamada mantém o estado da primeira, sem nova aplicação financeira nem duplicação da trilha do mesmo ato. Resposta da segunda chamada: regra nao decide. | Executar novamente as edições, os efeitos financeiros ou a trilha a cada confirmação. | borda |
| B07 — cruza meia-noite | Aval A2: “troca cruzando meia-noite”; `app/docs/CORTES.md:187`; contrato dos marcos em `app/escala/servico_jornada.py:62` | Troca completa confirmada. `TipoEscala.hora_inicio=19:00` (`app/escala/models.py:50`), `hora_fim=07:00` (`app/escala/models.py:51`). `Batida.colaborador=substituto` (`app/ponto/models.py:22`), `timestamp=2026-11-03 19:00 / 2026-11-04 07:00` (`app/ponto/models.py:23`), `tipo=E/S` (`app/ponto/models.py:24`). | Marcos permanecem encadeados no turno iniciado em 03/11; dia assumido recebe tratamento normal com AN/intra e sem HE100 pela troca. | Cortar o turno na meia-noite e tratar a saída como outro dia de folga trabalhada. | borda |
| B08 — fronteira 20/21 | Aval A2: “troca […] cruzando competencia”; `app/docs/CORTES.md:216`; contrato da janela em `app/ponto/janelas.py:11` | Par confirmado com duas cartas. `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`). Datas dos lados em `CelulaDia.data=2026-10-20 / 2026-10-21` (`app/escala/models.py:1685`), pessoas em `colaborador` (`app/escala/models.py:1681`). | 20/10 pertence à competência 10, janela 21/09–20/10; 21/10 pertence à competência 11, janela 21/10–20/11. Cada lado conserva o efeito contratado, sem virar SOLO pela separação das competências. | Atribuir os dois lados à competência do mês civil ou filtrar o par somente pela competência da primeira data. | borda |
| B09 — meia-noite no corte | Aval A2: “troca cruzando meia-noite e cruzando competencia”; `app/docs/CORTES.md:187`; `app/escala/servico_jornada.py:62` | `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`). `Batida.colaborador=substituto` (`app/ponto/models.py:22`), `timestamp=2026-10-20 19:00 / 2026-10-21 07:00` (`app/ponto/models.py:23`), `tipo=E/S` (`app/ponto/models.py:24`). Par completo confirmado com duas cartas. | Continua sendo turno cruzando meia-noite e dia assumido normal, sem HE100 pela troca. Rateio de AN/intra entre competências: regra nao decide. | Reclassificar como HE100 a parcela após 00:00 por ter mudado a competência. | borda |
| B10 — virada do ano | `app/ponto/janelas.py:11`; `app/docs/CORTES.md:216`; `app/docs/CORTES.md:187` | Par confirmado. `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`); `CelulaDia.data=2026-12-20 / 2026-12-21` (`app/escala/models.py:1685`), `colaborador` dos respectivos lados (`app/escala/models.py:1681`). | 20/12 pertence à competência 12/2026; 21/12 pertence à competência 01/2027. Mantêm-se os efeitos do par. | Incrementar o mês sem avançar o ano ou usar dezembro para ambas as datas. | borda |
| B11 — vínculo trocado | Regra do vínculo do dia em `app/escala/alimentacao.py:231`; limite da vigência em `app/escala/alimentacao.py:272`; O10 em `app/docs/CORTES.md:187` | `CelulaDia.data=2026-10-20 / 2026-10-21` (`app/escala/models.py:1685`), `escala_geradora=vínculo antigo / novo` (`app/escala/models.py:1692`). Vínculo antigo: `data_fim=2026-10-20` (`app/escala/models.py:876`), `ativa=False` (`app/escala/models.py:877`); novo: `data_inicio=2026-10-21` (`app/escala/models.py:875`). | Consulta do vínculo de 20/10 retorna o antigo; de 21/10, o novo, conforme geradoras e vigências. Aceitar a troca entre vínculos diferentes: regra nao decide. | Resolver ambos os dias pelo vínculo atualmente ativo. | borda |
| B12 — vazio | Aval A2: “par incompleto (so um lado) = pedido, nao vale”; `app/docs/CORTES.md:187`: “ATO com par” | Conjunto de lados vazio: nenhum `CelulaDia` selecionado (modelo em `app/escala/models.py:1635`). Campos do formulário de dia cedido/colega: [nao verificado]. | Não existe troca confirmada válida sem par. Retorno, mensagem e eventual criação de pedido vazio: regra nao decide. | Tratar uma seleção vazia como confirmação bem-sucedida de troca. | borda |
| A01 — duas confirmações concorrentes | Aval A2: “segunda confirmacao do mesmo par = idempotente”; aval FATIA B: “em atomic”; `CLAUDE.md:175` | Duas confirmações simultâneas do mesmo par completo, com duas cartas. Mesmos `CelulaDia.colaborador` (`app/escala/models.py:1681`) e `data` (`app/escala/models.py:1685`). Identidade persistida do par: [nao verificado]. | Estado final equivale a uma confirmação; sem duplicar efeitos ou trilha do mesmo ato; nenhum meio par confirmado. Qual chamada vence e quais respostas retornam: regra nao decide. | Deixar ambas as requisições aplicarem o ato integralmente após lerem o mesmo estado inicial. | adversarial |
| A02 — falha entre os lados | Aval FATIA B: “escrevendo o PAR pelas duas edicoes de celula do escritor existente, em atomic, com trilha (usuario, antes/depois, motivo)” | Par com `CelulaDia.colaborador` (`app/escala/models.py:1681`), `data` (`app/escala/models.py:1685`) e `trabalha` (`app/escala/models.py:1686`); falha provocada depois da primeira edição e antes da segunda. Trilha: `LogAuditoria.usuario` (`app/core/models.py:295`), `valor_antes` (`app/core/models.py:300`), `valor_depois` (`app/core/models.py:301`), `descricao` (`app/core/models.py:299`). | Após a falha, as duas células mantêm os valores anteriores; não existe trilha afirmando um par concluído. Resposta e registro da tentativa falha: regra nao decide. | Confirmar a primeira edição antes de executar a segunda, deixando estado parcial persistido. | adversarial |
| A03 — legado aprovado SOLO | `app/docs/BACKLOG.md:1224`: censo corrigido de 16 SOLO e conversão por “DRY -> aval”; aval A2: “par incompleto […] nao vale” | Fixture derivada do censo fornecido: 16 `Ausencia`, com `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data_fim=None` (`app/ponto/models.py:751`), `status` distribuído em 12 aprovadas, 2 rejeitadas e 2 aguardando (`app/ponto/models.py:799`). `TipoAusencia.efeito='abona'` (`app/ponto/models.py:581`). Sem referência ao outro lado: [nao verificado]. | As 12 aprovações legadas e o efeito `abona` não comprovam pares confirmados. DRY não grava conversão; conversão depende do aval previsto. Destino individual das 16 linhas: regra nao decide. | Promover automaticamente todo SOLO aprovado a troca confirmada, sem par, cartas ou aval de conversão. | adversarial |
| A04 — colega sem vínculo no dia | `app/escala/alimentacao.py:209`; `app/docs/CORTES.md:187` | Colega sem célula geradora na data. `EscalaColaborador.colaborador=colega` (`app/escala/models.py:872`), `data_inicio=2026-10-01` (`app/escala/models.py:875`), `data_fim=None` (`app/escala/models.py:876`), `ativa=False` (`app/escala/models.py:877`). Data consultada: 03/11. | `vinculo_cobre_a_data` retorna falso: vínculo desativado sem fim declarado não cobre a data. Confirmar ou recusar a troca nessa situação: regra nao decide. | Considerar suficiente `data_inicio` anterior ao dia e ignorar a ausência de vigência declarada. | adversarial |
| A05 — dia cedido já trabalhado | `app/docs/CORTES.md:187`: “folga trocada sem desconto”; `app/docs/BACKLOG.md:1248`: “a troca NAO cria desconto novo” | `CelulaDia.colaborador=cedente` (`app/escala/models.py:1681`), `data=2026-11-03` (`app/escala/models.py:1685`). Batidas existentes: `Batida.colaborador=cedente` (`app/ponto/models.py:22`), `timestamp=03/11 07:00 / 19:00` (`app/ponto/models.py:23`), `tipo=E/S` (`app/ponto/models.py:24`). Tentativa de troca completa. | Aceitar ou recusar diante do trabalho já realizado: regra nao decide. A troca não pode introduzir desconto novo para resolver essa colisão. | Gerar falta ou outro desconto novo ao encontrar batidas no dia cedido. | adversarial |
| A06 — célula já editada | Aval FATIA B: “com trilha (usuario, antes/depois, motivo)”; `app/docs/CORTES.md:187` | `CelulaDia.origem='editada'` (`app/escala/models.py:1691`), `trabalha=True` (`app/escala/models.py:1686`), `dna` previamente alterado (`app/escala/models.py:1697`), `data=2026-11-03` (`app/escala/models.py:1685`). Tentativa de confirmar par completo. | Sobrescrever ou recusar a edição humana anterior: regra nao decide. Se a troca for efetivada, a trilha identifica usuário, estado anterior real, estado posterior e motivo. | Registrar como “antes” o template gerado, apagando da trilha o estado humano efetivamente substituído. | adversarial |
| A07 — competência exportada | Aval PROIBIDO: “tocar exportada sem !” | `FechamentoMensal.colaborador` (`app/ponto/models.py:405`), `mes=9` (`app/ponto/models.py:408`), `ano=2026` (`app/ponto/models.py:409`), `status='exportado'` (`app/ponto/models.py:410`). Tentativa de efetivar troca nessa competência sem `!`. | No escopo deste aval, nenhuma alteração da competência exportada é autorizada. Recusa automática, código HTTP e mensagem: regra nao decide. | Aplicar a troca em competência exportada usando a autorização comum da obra. | adversarial |
| A08 — S5b por colaborador | `app/docs/BACKLOG.md:1248`: “a troca NAO cria desconto novo” e “todo outro campo de todo colaborador da ZERO”; `app/docs/PROMPTS.md:341` | Dois colaboradores sintéticos, um com troca e outro de controle. Linhas `DiaPago.colaborador` (`app/ponto/models.py:315`), `data` (`app/ponto/models.py:319`), `horas_atraso` (`app/ponto/models.py:334`), `horas_saida_antecipada` (`app/ponto/models.py:335`), `horas_falta` (`app/ponto/models.py:336`), comparadas antes/depois, mantendo os demais fatos. | Nenhum colaborador recebe desconto novo atribuível à troca. Redução em outro colaborador não compensa aumento indevido no primeiro. | Validar apenas o total da frota, permitindo descontos positivos individuais cancelados por diferenças negativas. | adversarial |
| A09 — selo de confirmação inválida | Aval FATIA B: “selo: troca confirmada sem 2 anexos = VERMELHO”; `app/docs/CORTES.md:187` | Estado inconsistente apresentado ao selo: confirmação do par [nao verificado]; `Ausencia.documento` vazio em pelo menos um dos lados (`app/ponto/models.py:777`). | Selo VERMELHO, mesmo que o estado apresentado se declare confirmado. Forma exata do retorno do selo: regra nao decide. | Pintar verde apenas pelo indicador de confirmação, sem conferir as duas cartas. | adversarial |

- **regras que voce NAO encontrou escritas:** identidade formal do par, campos de ligação entre os lados e representação persistida de “pedido”/“confirmada”; procurados em `app/escala/models.py:1635`, `app/ponto/models.py:660` e `app/docs/CORTES.md:187`; definição específica: [nao verificado].
- **regras que voce NAO encontrou escritas:** aceitação ou recusa de troca com dia já trabalhado, edição humana anterior, colega sem vínculo ou vínculos diferentes; procuradas no contrato `app/docs/CORTES.md:187` e nas regras de vínculo `app/escala/alimentacao.py:231`; política específica O10: [nao verificado].
- **regras que voce NAO encontrou escritas:** vencedor, respostas e tratamento de conflito entre confirmações concorrentes de pares diferentes; procurados no aval FATIA B “em atomic” e em `CLAUDE.md:175`; política específica: [nao verificado].
- **regras que voce NAO encontrou escritas:** datas iguais, invertidas, inválidas, futuras, distância máxima entre os dias e mensagem para entrada vazia; procuradas em `app/docs/CORTES.md:187`; critérios específicos: [nao verificado].
- **regras que voce NAO encontrou escritas:** quantias exatas de AN/intra, rateio do turno entre competências e demais efeitos financeiros de um dia cedido já trabalhado; procurados em `app/docs/CORTES.md:187` e `app/docs/BACKLOG.md:1248`; definição específica: [nao verificado].
- **regras que voce NAO encontrou escritas:** códigos HTTP, textos de recusa, validação de autenticidade/unicidade das cartas e destino individual das 16 linhas legadas; procurados em `app/docs/BACKLOG.md:1224` e `app/docs/CORTES.md:187`; definição específica: [nao verificado].

## 06/11 09:17 · sessao 07958c9b883b · LEI-FIO-MUDO-B · verificador · origem=Code

**Parecer: a remoção resolve o callback específico, mas a obra está incompleta para garantir a lei em todo o fluxo de abertura.** Permanecem reabertura automática alcançável pelo renascimento da cobrança, alarme F2 incompatível com a sobra agora permitida e lacunas relevantes no R08f. [app/chamados/models.py:1495; app/chamados/models.py:1625; app/chamados/management/commands/auditar_invariantes_chamados.py:1761; app/chamados/tests/test_c20_fio_mudo.py:247]

Execução dos testes, correspondência da cópia com o HASH/HEAD informado, quantidade afetada em produção e ocorrência efetiva de push: **[nao verificado]**. [nao verificado]

**1. O que está errado ou incompleto**

- **A proteção ficou na adoção, mas não cobre todo `abrir`.** O detector procura cobrança por módulo/data sem filtrar status; `abrir` pode encontrar encerrado e fazê-lo renascer; `reabrir` chama `ressuscitar_perguntas` **antes** da adoção. Essa ressuscitação ainda reabre disputas FECHADAS. É caminho condicional aos guardas existentes, não efeito obrigatório de qualquer abertura. [app/ponto/management/commands/detectar_ausencias.py:693; app/chamados/models.py:974; app/chamados/models.py:1073; app/chamados/models.py:1495; app/chamados/models.py:1625]
- **O F2 ainda alarma o estado que a lei aceita.** Seleciona pergunta viva, não validada/materializada, em disputa fechada de colaborador ativo, e termina com `ALARME`/exit 2. Continua agendado; o teste antigo exige a violação. Isso contradiz o contador escolhido pelo delta, cujo dono é ADMIN e que expressamente não alarma. [app/chamados/management/commands/auditar_invariantes_chamados.py:1761; app/chamados/management/commands/auditar_invariantes_chamados.py:1853; app/config/crons.py:542; app/chamados/tests/test_hx_inv_fio.py:75; app/chamados/services/vigia_fio.py:740]
- **“Só Validar” não está demonstrado para todo o modal.** O template ainda oferece “Reabertura administrativa” ao staff/admin_ti, além das ações da pergunta; o backend dessa ação reabre e envia FCM. No chamado novo que apenas herda a disputa, o backend procura uma disputa cujo próprio container seja aquele chamado; pelo container antigo, a ação continua disponível. É comportamento anterior deixado pelo delta, não reabertura automática da adoção. [app/templates/chamados/partials/modal_fio.html:377; app/templates/chamados/partials/modal_fio.html:385; app/chamados/services/acoes_disputa.py:252; app/chamados/services/acoes_disputa.py:256; app/chamados/services/acoes_disputa.py:283]
- **A pergunta sem resposta não ganha uma saída “Validar”.** O modal mostra “Aguardando resposta do colaborador”; a validação recusa `respondida_em` ausente. O R08f evita esse caso preenchendo a resposta. O destino operacional dessa classe sob a lei literal está **[nao verificado]**. [app/templates/chamados/partials/modal_fio.html:314; app/chamados/services/validacao.py:363; app/chamados/tests/test_c20_fio_mudo.py:247]
- **A docstring dos mocks promete cobertura maior que a existente**, e o topo do RELATO ainda apresenta a decisão como pendente e o callback removido como existente. [app/chamados/tests/test_c20_fio_mudo.py:218; app/ponto/management/commands/processar_alertas_turno.py:251; app/docs/RELATO.md:13]

**2. Quem escreve e quem lê**

Abaixo distingo persistência, leitura e alteração apenas em memória; a FK reversa `perguntas_que_resolvem` também lê a relação `chamado_destino`. [app/chamados/models.py:2882]

| Alvo | Escritores/definição | Leitores/chamadores principais |
|---|---|---|
| `_adotar_vivas_na_abertura` | Definida em `app/chamados/models.py:434`; costura por `fio.reapontar_resolvedora` em `app/chamados/models.py:502`. | `abrir`: `app/chamados/models.py:1027`; `reabrir`: `app/chamados/models.py:1496`; chamadas diretas nos testes: `app/chamados/tests/test_c20_fio_mudo.py:188,316,333,460`. |
| Função removida `reabrir_continentes_das_reatadas` | A lápide registra sua remoção e a do chamador. | Restam referências nas lápides e no RELATO desatualizado: `app/chamados/models.py:517`; `app/chamados/services/fio.py:899`; `app/docs/RELATO.md:13`. |
| `DisputaSupervisao.fechada_em` | `fechar`: `app/chamados/models.py:1955`; `reabrir_admin`: `app/chamados/models.py:2157`; `reabrir_sistema`: `app/chamados/models.py:2181`. | Censo abaixo; a property `aberta` lê em `app/chamados/models.py:1859`. |
| `DisputaSupervisao.resolucao` | `fechar`: `app/chamados/models.py:1957`; `reabrir_admin`: `app/chamados/models.py:2159`; `reabrir_sistema`: `app/chamados/models.py:2183`; `limpar_resolucao_residual`: `app/chamados/models.py:2267`. | Idempotência/limpeza: `app/chamados/models.py:1915,2264,2266`; ocorrência no fio: `app/chamados/signals.py:45`; fallback do materializador: `app/chamados/services/materializacao.py:1253`; snapshot: `app/chamados/management/commands/reabrir_orfas_enterradas.py:132`. |
| `PerguntaDisputa.chamado_destino` | Nascimento: `app/chamados/models.py:2571`; reaponte de existente no gate de identidade: `app/chamados/models.py:2544`, salvo em `:2568`; `reapontar_resolvedora`: `app/chamados/services/fio.py:845`, salvo em `:847`. | Censo abaixo; autoridade da âncora: `app/chamados/services/fio.py:117`. |

**Há dois lugares que reapontam pergunta existente**, embora a docstring de `reapontar_resolvedora` reivindique exclusividade. É anterior ao delta: o gate `PerguntaDisputa.abrir` também escreve e salva o destino de uma identidade encontrada. [app/chamados/services/fio.py:787; app/chamados/services/fio.py:845; app/chamados/models.py:2544; app/chamados/models.py:2568]

As atribuições em `models.py:350` e `fio.py:773,777` são atualização/simulação em memória; não são persistência adicional desse campo. [app/chamados/models.py:349; app/chamados/services/fio.py:772]

Censo dos acessos executáveis a **`fechada_em`**, incluindo filtros, ordenação e carregamento; referências agrupadas por arquivo:

```text
app/chamados/models.py:184,1625,1831,1834,1841,1859,1914,1918,2041,2146,2175,2232,2264
app/chamados/signals.py:28,106,130
app/chamados/reconciliador.py:411,589,600,601,1038,1149,1150,1487
app/chamados/views.py:471,495,512,530,911,1329
app/chamados/views_cobrar.py:161,164
app/chamados/relatorios_disputa.py:41
app/chamados/juizes.py:2080
app/chamados/services/fio.py:131,135,184,596,629
app/chamados/services/relavra_via.py:181,244
app/chamados/services/acoes_disputa.py:252,343
app/chamados/services/becos.py:27,51,54
app/chamados/services/competencia_trancada.py:151,196
app/chamados/services/disputa_emissao.py:38,468,936,1157,1311,1575,1621,1645,1647,1695
app/chamados/services/materializacao.py:567,1241
app/chamados/services/validacao.py:140,196,199,201
app/chamados/services/acoes_chamado.py:336
app/chamados/services/fila_por_causa.py:225,259,262
app/chamados/services/vigia_fio.py:748
app/chamados/services/cobranca.py:230
app/chamados/services/regularizacao_ext.py:59
app/chamados/management/commands/fechar_becos_disputa.py:32
app/chamados/management/commands/auditar_chamados_orfaos.py:40
app/chamados/management/commands/encerrar_perguntas_com_fato.py:82
app/chamados/management/commands/auditar_invariantes_chamados.py:536,592,806,1762
app/chamados/management/commands/reabrir_orfas_enterradas.py:96,131,154,155
app/chamados/management/commands/auditar_disputas.py:72,87,136,152,167
app/api/foto_recebida.py:62
app/api/views.py:1008,1030,2694
app/colaboradores/painel_cell.py:32
app/colaboradores/signals.py:73
app/colaboradores/services/situacional.py:117
app/colaboradores/services/drawer.py:70
app/colaboradores/services/desligamento.py:139
app/escala/signals.py:157
app/ponto/services/entrada_sincronizada.py:56
app/ponto/management/commands/escalonar_chamados_supervisao.py:58,149
app/ponto/management/commands/reconciliar_fantasmas.py:214
app/ponto/management/commands/alertar_disputas_pendentes.py:34
app/templates/chamados/partials/modal_fio.html:374
bin/censo_folha.py:28
```

Leituras indiretas por **`aberta`**, cuja fonte é `fechada_em`: montagem/flags do fio, representação do modelo e escolha do ramo no modal. [app/chamados/services/fio.py:191,193,197,213,231,338; app/chamados/models.py:2279; app/templates/chamados/partials/modal_fio.html:321]

Censo dos acessos executáveis diretos a **`chamado_destino`**:

```text
app/chamados/models.py:287,293,294,328,344,345,349,419,501,2532
app/chamados/juizes.py:743,803,817,1579,1706,1750,1752,2030
app/api/views.py:1010,1045,1046,2574,2590,2592
app/chamados/views_cobrar.py:165
app/chamados/reconciliador.py:641,1040,1485
app/colaboradores/services/calendario.py:474
app/ponto/management/commands/reconciliar_grade.py:460
app/ponto/management/commands/reconciliar_fantasmas.py:212
app/chamados/services/vigia_carimbo.py:103
app/chamados/services/materializacao.py:339,526,563,564,686,764,964,1046,1075,1077,1167,1188,1190
app/chamados/services/validacao.py:795
app/chamados/services/pergunta_no_app.py:140,163,166
app/chamados/services/limbo_folgas.py:203,238,239
app/chamados/services/declaracao_texto.py:314
app/chamados/services/acoes_chamado.py:357
app/chamados/services/disputa_emissao.py:210,404,909,929,1244,1573,1653
app/chamados/services/explicador.py:122
app/chamados/services/competencia_trancada.py:108,118,140
app/chamados/services/raiox.py:55,60,135
app/chamados/services/becos.py:30,46,47
app/chamados/services/janela_colab.py:64,65
app/chamados/services/acoes_disputa.py:350
app/chamados/services/fio.py:80,81,117,135,223,224,327,392,393,772
app/chamados/services/relavra_via.py:99,101,154
app/chamados/services/historico.py:96
app/chamados/management/commands/auditar_invariantes_chamados.py:526,532,537,588,589,595,856,892,893,896,1159,1573
app/chamados/management/commands/reabrir_orfas_enterradas.py:69
app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:87,117,119
app/chamados/management/commands/relavrar_via_i6.py:56
app/chamados/management/commands/auditar_disputas.py:151
app/chamados/management/commands/lavrar_perguntas_stale.py:57,86
app/chamados/management/commands/backfill_veredito_lavrado.py:101
```

Leituras pela **FK reversa**, relevantes para não perder consumidores sem o nome literal do campo:

```text
app/chamados/models.py:1528,1593
app/chamados/services/fio.py:938,940
app/chamados/services/disputa_emissao.py:695
app/chamados/management/commands/auditar_invariantes_chamados.py:1042,1043,1627,1649
app/chamados/management/commands/disparar_perguntas_competencia.py:147,148
app/ponto/supra_juiz.py:547
app/ponto/management/commands/supra_juiz.py:192,215
app/ponto/management/commands/emitir_furo_retroativo.py:83,98
app/ponto/services/cartorio.py:118,1339,1594,1614
```

Fornecem o destino à porta de nascimento os serviços de declaração e emissão; não são escritores independentes da coluna. [app/chamados/services/declaracao_texto.py:326,615; app/chamados/services/disputa_emissao.py:395,776,1201; app/chamados/models.py:2571]

As migrações declaram esquema, não atualização de dados: `fechada_em/resolucao` em `0013`, destino em `0016`. [app/chamados/migrations/0013_disputasupervisao.py:24,25; app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:18]

Há escritores crus de **fixture**, incluindo o novo `update` e os testes de signals/ressuscitação; os selos inspecionam escritores e estado. Isso não constitui segundo escritor de produção. [app/chamados/tests/test_c20_fio_mudo.py:245; app/chamados/tests/test_signals.py:62,73,94,107,121,141,153,165,178,193,213,230,250,268,290; app/chamados/tests/test_selo_i8_fio_solto.py:201; app/ponto/tests/test_hx_cartorio_ressuscita.py:87; app/chamados/tests/test_selo_resolucao_um_escritor.py:44; app/core/tests/test_contract_escritores_estado.py:61]

A classificação semântica exaustiva de **todos os acessos dentro de todos os testes**, além dos identificados acima, permanece **[nao verificado]**; não equiparo busca lexical a censo de escritores/leitores. [nao verificado]

**3. Caminhos de reabertura e push; alcance pela abertura** [nao verificado]

| Caminho | Efeito | Alcançado pelo fluxo perguntado? |
|---|---|---|
| `reapontar_resolvedora` | Salva destino/lavra; o signal agenda fechamento de beco, que exclui FECHADA. [app/chamados/services/fio.py:845; app/chamados/signals.py:191; app/chamados/services/becos.py:27] | **Sim**, diretamente pela adoção; não reabre nem envia push nesse caminho. [app/chamados/models.py:502] |
| `migrar_resolvedoras_vivas` | Reaponta/retrata; sem reabertura direta da disputa. [app/chamados/services/fio.py:944,954] | Pode ser alcançado pelo ramo “irmão vivo” de `reabrir`, que retorna antes da adoção. [app/chamados/models.py:1465,1480] |
| `ressuscitar_perguntas` | Descarimba em lote e reabre FECHADAS por `reabrir_sistema`; exige materializada, sem texto, marco/C6 permitindo. [app/chamados/models.py:1528,1531,1546,1552,1598,1625] | **Sim**, no renascimento via `abrir → reabrir`, antes da adoção. [app/chamados/models.py:974,1073,1495] |
| Ressuscitação pelo cartório | Cobrança viva sem fio e sem resposta pode entrar na mesma porta. [app/ponto/services/cartorio.py:1591,1595,1597,1622] | **Sim pela vigia**, condicional à seleção; não é consequência obrigatória do reapontamento. [app/ponto/services/vigia_de_hora.py:131] |
| Fábrica de disputa após abertura | Usa `notificar=True`; pode enviar push se a disputa permanecer aberta. Disputa vazia pode fechar e retornar antes do envio. [app/chamados/services/disputa_emissao.py:876,1112,1116,1123] | **Sim**: detector chama a fábrica após coletar órfãos. O filtro para marcos da ata permite passagem mesmo existindo pergunta não materializada. [app/ponto/management/commands/detectar_ausencias.py:790; app/chamados/services/disputa_emissao.py:925,928] |
| `desfazer_carimbo_sem_lastro` | Default permite reabrir FECHADA; chamada do julgamento passa `reabrir_fechada=False`. [app/chamados/services/relavra_via.py:185,244; app/chamados/services/fabrica_da_celula.py:166] | Não pela adoção diretamente; o uso protegido do julgamento não reabre FECHADA. [app/chamados/models.py:502; app/chamados/services/fabrica_da_celula.py:166] |
| Validação recusada | Default reabre; fio do admin passa `sem_reabrir` quando fechada. [app/chamados/services/validacao.py:199; app/chamados/services/fio.py:596,605] | Não pela adoção; depende de validação posterior. [app/chamados/services/validacao.py:425] |
| Ausência rejeitada | Devolve perguntas absorvidas e reabre suas disputas. [app/ponto/signals.py:261; app/chamados/services/materializacao.py:658,661] | Ato próprio de ausência; não chamado pela adoção. [app/chamados/models.py:502; app/chamados/services/materializacao.py:638] |
| `reabrir_orfas_enterradas --apply` | Reabre fantasma e disputa fechada. É comando de uma vez. [app/chamados/management/commands/reabrir_orfas_enterradas.py:147,155; app/config/crons.py:785] | Não chamado pela adoção. [app/chamados/models.py:502] |
| Reabertura administrativa | Reabre disputa, cria pergunta e envia FCM. [app/chamados/services/acoes_disputa.py:256,259,283] | Ação humana posterior, não automática na adoção. [app/chamados/services/acoes_disputa.py:242,245,250] |
| Desarquivar/reabrir competência | Chamam `Chamado.reabrir`; desarquivar pela tranca também ressuscita depois de levantar o véu. [app/chamados/views_arquivar.py:70,86; app/chamados/services/competencia_trancada.py:250,265] | Outros atos; podem alcançar a ressuscitação sobrevivente. [app/chamados/models.py:1495] |
| `reabrir_por_colab` | **Recusa FECHADA**; reabre apenas questionário de disputa aberta. [app/chamados/models.py:2232,2240] | Não contorna o fechamento nesse caso. [app/chamados/models.py:2233] |

**Existe ainda o push posterior do cron diário:** ele seleciona disputas abertas e envia `push_colaborador`, sem exigir pergunta pendente/C6 nesse laço. Portanto uma reabertura sobrevivente habilita cobrança posterior que o R08f não executa; o agendamento é 08h. [app/ponto/management/commands/alertar_disputas_pendentes.py:33,48,59; app/config/crons.py:745]

Os signals de `DisputaSupervisao` tratam fechamento/materialização/retomada; o de pergunta agenda beco; o de chamado pode entrar no cartório. Não há neles uma nova reabertura direta inserida pelo delta. [app/chamados/signals.py:18,97,124,164,191; app/chamados/models.py:516]

**4. Órfãos e comportamento antigo**

- **Referência operacional órfã:** topo de `RELATO.md` ainda cita a função removida, callback e espera pela escolha A/B. [app/docs/RELATO.md:13]
- **Lápides intencionais:** as referências em `models.py` e `fio.py` declaram explicitamente a remoção; não são imports quebrados. [app/chamados/models.py:516; app/chamados/services/fio.py:899]
- **Regra antiga ampla:** MAPA e docstring de `relavra_via` dizem que ressuscitar em FECHADA religa a disputa. Ainda descrevem código sobrevivente, mas não explicitam a exceção da adoção muda. [app/docs/MAPA/chamado.md:89; app/chamados/services/relavra_via.py:69; app/chamados/services/relavra_via.py:244]
- **Outro texto impreciso:** `devolver_validacao_recusada` descreve reabertura da fechada, embora possua a exceção `reabrir=False`. [app/chamados/services/validacao.py:171,197]
- **Teste/contador incompatível:** F2 e seu teste continuam exigindo alarme da viva em fechada; não são apenas documentação histórica. [app/chamados/management/commands/auditar_invariantes_chamados.py:1761; app/chamados/tests/test_hx_inv_fio.py:75]
- **Teste do comportamento sobrevivente:** o selo I8 exige reabertura após desfazer carimbo; não ficou importando função apagada. [app/chamados/tests/test_selo_i8_fio_solto.py:192,205] [nao verificado]
- **Contador escolhido não ficou órfão:** conta pelo C6, tem dono ADMIN, não alarma e é consumido por `vigiar_fio_morto`. O GLOSSARIO conserva referência a ele. [app/chamados/services/vigia_fio.py:737,748,755; app/chamados/management/commands/vigiar_fio_morto.py:174,225; app/docs/GLOSSARIO.md:28]

**5. O R08f morde?**

**Morde a volta do callback original nesse cenário:** exige reapontamento, preservação conjunta de fechamento/resolução e executa callbacks antes das assertivas. [app/chamados/tests/test_c20_fio_mudo.py:253,258,260]

Não certifica os seguintes mutantes plausíveis:

| Mutante | Por que escaparia deste teste | [nao verificado]
|---|---|
| Reabrir somente pergunta **sem resposta** | O teste força `resposta_colab/respondida_em`. [app/chamados/tests/test_c20_fio_mudo.py:247] | [nao verificado]
| Reabrir em renascimento, ressuscitação ou cron posterior | R08f exercita nascimento pelo vigia; R08r testa `reabrir`, mas sem fechamento/push como invariantes. [app/chamados/tests/test_c20_fio_mudo.py:150,168,253] | [nao verificado]
| Mandar push por wrapper padrão | A fixture não configura usuário/token; wrappers podem retornar antes dos mocks inferiores. [app/chamados/tests/test_pergunta_do_dia_em_curso.py:34; app/chamados/fcm_utils.py:104; app/chamados/push_utils.py:116] |
| Colocar a pergunta no **card antigo** do admin | O teste achata perguntas de todos os cards e verifica presença global no HTML; não exige que esteja sob `ch.pk`. [app/chamados/tests/test_c20_fio_mudo.py:266,276; app/chamados/services/fio.py:236] |
| Exibir `corrigir_resposta` indevidamente | Fixture é `escolha`; Corrigir pertence ao ramo `hora`; teste só proíbe `recusar_pergunta`. [app/chamados/tests/test_c20_fio_mudo.py:65,278; app/templates/chamados/partials/modal_fio.html:295] |
| Reabrir/enviar push no **POST Validar** | Só faz GET; mocks e verificação do fechamento já terminaram. Remover `sem_reabrir` da delegação não seria exercitado. [app/chamados/tests/test_c20_fio_mudo.py:253,260,275; app/chamados/services/fio.py:605] |

**Os mocks não cobrem todos os canos da árvore.** Há `pywebpush.webpush` direto ao colaborador em comandos de alertas, fora dos três alvos. A chamada desses comandos pela adoção não foi demonstrada; sua execução nesse fluxo é **[nao verificado]**. [app/chamados/tests/test_c20_fio_mudo.py:224; app/ponto/management/commands/processar_alertas_turno.py:247,251,288; app/ponto/management/commands/processar_alertas_avancados.py:195,212]

Imports de wrappers por outros módulos não furam, por si, 

[truncado em 20000 de 21456 caracteres]

## 06/11 09:16 · sessao 5a281d793e06 · - · - · origem=Ronald (tmux)

Q5 — `troca_turno` e `troca_de_plantao`

| Item | Escritores / leitores / efeito |
|---|---|
| `troca_turno` pertence a `Justificativa.tipo` | Choice explícito em `app/ponto/models.py:126`; migration de schema `app/ponto/migrations/0019_tipos_justificativa.py:16`; opção web `app/templates/ponto/justificativa_form.html:43`; opção API `app/api/views.py:2070`; rótulo no histórico API `app/api/views.py:2099`, consumido em `app/api/views.py:2111`. |
| Quem grava `Justificativa.tipo` informado pelo humano | API lê `tipo` e atualiza/cria a pendente em `app/api/views.py:1955`, `app/api/views.py:2001`, `app/api/views.py:2007`; web faz equivalente em `app/ponto/views.py:1591`, `app/ponto/views.py:1625`, `app/ponto/views.py:1635`. Esses caminhos recebem o valor enviado; não há tratamento específico de troca nesses trechos. |
| Outros escritores capazes de editar a justificativa | Django admin registra `JustificativaAdmin(admin.ModelAdmin)` em `app/ponto/admin.py:18`, com tipo e status na listagem em `app/ponto/admin.py:20`; não herda `SoLeitura`, diferentemente de Ausencia em `app/ponto/admin.py:37`. |
| Quem decide a aprovação | Individual: view delega em `app/ponto/views.py:1915`; serviço abre atomic/lock em `app/ponto/services/justificativa.py:125`, grava status/usuário/data em `app/ponto/services/justificativa.py:190`, invalida fechamento em `app/ponto/services/justificativa.py:199`. Lote: atomic por item em `app/ponto/views.py:1792`, status/carimbos em `app/ponto/views.py:1796`, invalidação em `app/ponto/views.py:1804`. Materialização retroativa também aprova em `app/ponto/services/justificativa.py:368`. |
| Pode criar batida? | A aprovação individual cria S manual se há `saida_informada` **e** entrada, sem condicionar ao tipo, em `app/ponto/services/justificativa.py:137`, `app/ponto/services/justificativa.py:172`. Lote exige saída informada e ausência de batida gerada em `app/ponto/views.py:1808`. API que anuncia `troca_turno` marca `requer_saida=False` em `app/api/views.py:2070`; seu POST não atribui saída em `app/api/views.py:2007`. |
| Efeito financeiro de justificativa aprovada | **Não possui campo `efeito='abona'`.** Fechamento lê qualquer justificativa aprovada por data, sem filtrar tipo, para `datas_justificadas`: `app/ponto/services/fechamento.py:651`. Serviço do espelho faz equivalente em `app/ponto/services/efeito_ausencia.py:36`. Motor exclui dessas datas as faltas injustificadas e conta abono se não houve trabalho em `app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2606`; `dsr_ok = not faltas_injustificadas` em `app/ponto/motor_calculo_v2.py:2605`. Não escreve o par de células nesses caminhos: aprovação grava Justificativa/batida em `app/ponto/services/justificativa.py:172`, `app/ponto/services/justificativa.py:190`. |
| SLA de `troca_turno` | Prazo 3 dias em `app/ponto/catalogo/justificativas.py:24`, função em `app/ponto/catalogo/justificativas.py:29`; leitura pela fila em `app/ponto/views.py:1726` e cron em `app/ponto/management/commands/alertar_justificativas_sla.py:19`. É prazo do DP, não cálculo monetário, conforme contrato em `app/ponto/catalogo/justificativas.py:9`. |
| `troca_de_plantao` | O literal só apareceu em documentação na busca da cópia, não em implementação runtime nem migrations; origem histórica exata do cadastro fornecido pelo usuário = **[nao verificado]**. O mecanismo atual aceita códigos cadastrados: valida `codigos_validos()` em `app/ponto/services/ausencia.py:193`, exclui `tipo` do full_clean por sua autoridade ser o catálogo em `app/ponto/services/ausencia.py:361`. |
| Escritor de `TipoAusencia` | View aceita nome, efeito e flags em `app/ponto/views_tipos_ausencia.py:61`, `app/ponto/views_tipos_ausencia.py:67`; chama `gravar_tipo_ausencia` em `app/ponto/views_tipos_ausencia.py:86`, `app/ponto/views_tipos_ausencia.py:94`. Porta grava existente ou cria em `app/ponto/services/ausencia.py:1285`. Código imutável nessa edição em `app/ponto/views_tipos_ausencia.py:84`. |
| Escritor de Ausencia desse tipo cadastrado | Criar recebe código validado e monta tipo, datas, minutos, documento, status e autor em `app/ponto/services/ausencia.py:341`; API chama em `app/api/views.py:1820`, admin em `app/ponto/views.py:2415`, PWA em `app/ponto/views.py:2536`. Edição genérica grava tipo/datas/minutos em `app/ponto/services/ausencia.py:497`; decisão passa por `mudar_estado` em `app/ponto/services/ausencia.py:895`. |
| De quem é `efeito='abona'` e quem o lê | É de **TipoAusencia**, campo em `app/ponto/models.py:581`; `Ausencia.efeito` consulta efeito vivo em `app/ponto/models.py:840`. Catálogo lê cadastro ativo em `app/ponto/catalogo/ausencias.py:249`, `efeito_vigente` devolve a primeira posição em `app/ponto/catalogo/ausencias.py:293`. Portanto o nome customizado não precisa de ramo específico. |
| Efeito financeiro de Ausencia aprovada com efeito abona | Cobertura para folha filtra só aprovada em `app/ponto/turnos.py:2514`. Fechamento lê `a.efeito` e inclui dia em `datas_justificadas` em `app/ponto/services/fechamento.py:684`; pagas parciais entram como crédito de minutos em `app/ponto/services/fechamento.py:664`. Ausencia inteira abonada alimenta teto que zera pontualidade em `app/ponto/turnos.py:2681`, selecionada por efeito em `app/ponto/turnos.py:2689`. Export inclui tipos abonados, menos exceções, para rubrica 8932 em `app/folha/export.py:193`, `app/folha/export.py:201`; exceções em `app/ponto/catalogo/ausencias.py:290`. |
| Migrations de dados | Seed `TipoAusencia` contém os 17 códigos em `app/ponto/migrations/0052_seed_tipoausencia.py:9`, grava por `get_or_create` em `app/ponto/migrations/0052_seed_tipoausencia.py:33`; não contém `troca_de_plantao`. Backfill genérico converte antigas pendentes segundo presença de **um documento**/tipos exigentes em `app/ponto/migrations/0057_a4_backfill_pendente.py:30`, `app/ponto/migrations/0057_a4_backfill_pendente.py:35`, com trilha em `app/ponto/migrations/0057_a4_backfill_pendente.py:44`. Aplicação em prod dessas migrations = **[nao verificado]**. |

Não inferir do código quantas trocas foram executadas, quem as cadastrou ou anexos existentes: **[nao verificado]**; os 16/12/2/2 e todos SOLO/abona são somente o anexo raspado do usuário **[nao verificado por arquivo da cópia]**.

Q6 — cartas e dois anexos

| Item | Evidência |
|---|---|
| Onde chega o arquivo | `Ausencia.documento` é **um** `FileField`, `upload_to="ausencias/documentos/"`, opcional: `app/ponto/models.py:777`. `documento_referencia` aponta para primeira Ausencia de intervalo, não é segundo anexo: `app/ponto/models.py:791`. |
| Portas de envio inicial | API `request.FILES.get('documento')` em `app/api/views.py:1775`, repassado em `app/api/views.py:1822`; admin em `app/ponto/views.py:2393`, `app/ponto/views.py:2418`; PWA em `app/ponto/views.py:2480`, `app/ponto/views.py:2538`. Inputs únicos em `app/templates/ponto/lancar_ausencia.html:85`, `app/templates/ponto/submeter_ausencia.html:52`. |
| Envio posterior | API própria verifica colaborador e estado aberto em `app/api/views.py:3092`, `app/api/views.py:3102`, recebe um arquivo em `app/api/views.py:3106`, chama porta em `app/api/views.py:3130`. Admin aceita anexo/substituição em qualquer status em `app/ponto/views.py:2133`, chama mesma porta em `app/ponto/views.py:2150`. |
| Escritor do anexo posterior | `anexar_documento` **substitui** `ausencia.documento`, salva esse campo e registra antes/depois/ator em `app/ponto/services/ausencia.py:840`, `app/ponto/services/ausencia.py:854`. Se aguardava documento, muda para aguardando decisão em `app/ponto/services/ausencia.py:857`. |
| Leitor para download | Resolve `documento_referencia or a`, abre o arquivo da raiz e devolve `FileResponse` em `app/ponto/views.py:2115`, `app/ponto/views.py:2122`, `app/ponto/views.py:2127`. |
| Exigência existente | Cadastro tem booleano `exige_documento` em `app/ponto/models.py:583`; `estado_inicial(tipo, tem_documento)` decide por booleano e não cardinalidade em `app/ponto/services/ausencia.py:763`, `app/ponto/services/ausencia.py:775`. |
| Exige 2 documentos? | As portas verificadas recebem um arquivo, o modelo possui um campo e o gate só testa presença. Não há validação de “carta do cedente + carta do substituto” nesses mecanismos: `app/ponto/models.py:777`, `app/ponto/services/ausencia.py:775`, `app/ponto/services/ausencia.py:840`. Outra implementação de exigência de 2 anexos: **[nao verificado]**. |
| Aprovação sem documento | Gate de UI permite confirmação explícita sem lastro em `app/ponto/services/ausencia.py:612`; máquina não permite ir diretamente de aguardando documento para aprovada: `app/ponto/services/ausencia.py:752`. Admin pode criar já aprovada em `app/ponto/views.py:2420`. Não confundir esses caminhos com garantia obrigatória de dois documentos. |

Q7 — conexos relevantes

- **Falta já aprovada/minutos persistidos:** `Ausencia.save` só deriva minutos se tipo falta, aprovada e minutos vazio; soma `minutos_previstos_do_dia` em `app/ponto/models.py:877`, `app/ponto/models.py:885`. Trocar célula posteriormente não rederiva automaticamente um valor positivo nesse método; export de faltas continua consultando Ausencias cujo catálogo diz desconta em `app/folha/export.py:179`. Isso é fonte separada a conferir na conversão do par.
- **Justificativa apaga cobrança visual sem alterar tipo de dia:** grade consulta todas aprovadas em `app/escala/utils.py:1218`; contrato comenta limpeza de furo sem mudar tipo em `app/escala/utils.py:1213`. Post-save rejulga o dia em `app/ponto/signals.py:161`.
- **Relavra por fato novo:** cartório observa criado/atualizado/aprovado de Ausencia e Justificativa e `CelulaDia.editada_em`/`regenerada_em` em `app/ponto/services/cartorio.py:355`, `app/ponto/services/cartorio.py:359`, `app/ponto/services/cartorio.py:365`.
- **Status de Ausencia tem outro efeito além da folha:** folha só aprovada; cobrança aceita qualquer não rejeitada em `app/ponto/turnos.py:2514`. Converter tipo para “pedido” mexe também nesse consumidor, se continuar modelado como Ausencia.
- **Cache do cadastro:** runtime tem TTL de 60s em `app/ponto/catalogo/ausencias.py:258`, fonte viva/fallback em `app/ponto/catalogo/ausencias.py:278`; save/delete de TipoAusencia invalida cache do processo em `app/ponto/models.py:1235`. Não atribuir automaticamente efeito novo a todo histórico sem conferir leitores que consultam o cadastro atual.
- **Escritores paralelos do status de Justificativa:** individual em `app/ponto/services/justificativa.py:190`; lote em `app/ponto/views.py:1796`; retroativa em `app/ponto/services/justificativa.py:368`; rejeição individual em `app/ponto/services/justificativa.py:268`; rejeição lote em `app/ponto/views.py:1875`; admin editável em `app/ponto/admin.py:19`. Regulação retorno também pode pôr pendente a linha encontrada, sem trocar seu tipo, em `app/ponto/services/regularizacao.py:102`.
- **OrdemSubstituicao/AgendaDia são entidades existentes:** escritores declarados em `app/core/portas.py:475`, `app/core/portas.py:478`; cobertura cria ordem em `app/ponto/services/cobertura.py:100`; agenda cria dia em `app/escala/services/agenda.py:17`. Não examinei integralmente seu efeito em folha: **[nao verificado]**.

## 06/11 09:16 · sessao 1101f91bfa73 · - · - · origem=Ronald (tmux)

Q1 — A premissa de “escritor humano existente de `origem='editada'`” não corresponde à cópia: os campos existem, mas a catraca declara **sem escritor de produção** `editada_em` e `editada_por`; o teste varre produção e exige que continuem sem escritor até a obra própria. Evidências: `app/escala/models.py:1690`, `app/escala/models.py:1732`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:20`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:72`, `app/escala/tests/test_d3_guarda_da_linha_celula.py:179`. O serviço de fase também declara literalmente “escritor de producao: nenhum declarado hoje”: `app/escala/services/fase_12x36.py:161`.

| Escritor vivo, distinto de edição humana da previsão | O que grava / trilha / atomic / chamada |
|---|---|
| `gerar_celulas_periodo` | Cria `colaborador,data,trabalha,origem='gerada',escala_geradora,dna`; atualiza previsão gerada e pula editada; não possui `atomic` próprio no corpo. `app/ponto/portas/celula.py:595`, `app/ponto/portas/celula.py:633`, `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:650`. Command reexporta: `app/ponto/management/commands/gerar_celulas.py:13`. |
| `regenerar_celulas_vinculo` | Mesmo nascimento `gerada`; reescrita só gerada; guarda `dna_anterior,regenerada_em,regeneracoes`; `atomic` em :212. `app/ponto/portas/celula.py:212`, `app/ponto/portas/celula.py:253`, `app/ponto/portas/celula.py:257`, `app/ponto/portas/celula.py:265`. |
| `veredito_celula` / `CelulaDia.lavrar_veredito` | Ato humano existente do Resolver, mas **veredito**, não edição de `trabalha/dna/origem`. Serviço tem `atomic` :204; chama a lavra :330; modelo delega à porta. `app/ponto/services/veredito_celula.py:191`, `app/ponto/services/veredito_celula.py:204`, `app/ponto/services/veredito_celula.py:330`, `app/escala/models.py:1819`. A porta grava `veredito,veredito_via,veredito_em,protestos`, ata/status quando aplicável; trilha `ator,antes,depois` em evento. `app/ponto/portas/celula.py:764`, `app/ponto/portas/celula.py:811`, `app/ponto/portas/celula.py:815`, `app/ponto/portas/celula.py:819`. Save+outbox em atomic próprio :681; serviço também loga ações/erros com autor. `app/ponto/portas/celula.py:676`, `app/ponto/services/veredito_celula.py:334`. View: `app/ponto/views.py:3312`. |

**Duas células no mesmo atomic:** a identidade é apenas `(colaborador,data)`; não há unicidade que impeça dois colaboradores e datas distintas. A autoridade de escrita é o **módulo** `ponto/portas/celula.py`, não “uma célula por transação”: `app/escala/models.py:1833`, `app/core/portas.py:107`. Entretanto, **não há API humana de edição existente para invocar duas vezes**; chamar `veredito_celula` duas vezes não escreve os overrides pretendidos, pois as opções são os três vereditos e o serviço somente os lavra. `app/ponto/services/veredito_celula.py:18`, `app/ponto/services/veredito_celula.py:201`, `app/ponto/services/veredito_celula.py:330`. O gate exige que toda escrita permaneça numa operação declarada da porta; hoje são gerar/lavrar/carimbar. `app/ponto/portas/celula.py:66`, `app/ponto/tests/test_chokepoint_celula_gate.py:62`, `app/ponto/tests/test_chokepoint_celula_gate.py:69`.

**Admin Django:** `app/escala/admin.py:4` importa e registra só TipoEscala e EC; EC é `SoLeitura` em :13. `app/ponto/admin.py:2`–:48 registra Batida/Justificativa/Importacao/Alerta/Ausencia. Nenhum registro de CelulaDia encontrado na busca em admin/forms/serializers; existência de algum mecanismo dinâmico externo à cópia é [nao verificado].

**Escritores de fixtures/testes `editada`**, separados de runtime: [nao verificado]

- Atribuição+save: `app/escala/tests/test_celula_dia.py:73`, `app/escala/tests/test_celula_dia.py:98`, `app/escala/tests/test_celula_dia.py:358`; `app/colaboradores/tests/test_vinculo_regenera_celulas.py:39`; `app/ponto/tests/test_porta_celula_dia.py:247`.
- `create/update_or_create/update`: `app/escala/tests/test_selo_foto_folgas_relavra.py:104`, `app/escala/tests/test_sem_dia_pelo_juiz.py:46`, `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:97`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:134`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:180`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:225`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:242`.
- Helpers recebem origem editada e fazem create: `app/escala/tests/test_selo_celula_alem_do_vinculo.py:58` chamado em :103; `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:116` chamado em :128.
- Migration apenas cria schema/choices, sem escritor de dado editado: `app/escala/migrations/0030_celuladia.py:23`, :25, :27.

Q2 — Leitores e sobrevivência:

| Leitor | Comportamento |
|---|---|
| EC.eh_dia_trabalho | Consulta célula sem filtrar origem e retorna `cel.trabalha`; só sem célula cai na aritmética do vínculo. `app/escala/models.py:1087`, `app/escala/models.py:1094`. |
| EC.marcos_do_dia | Lê `dna.marcos` da célula, sem filtrar origem; se `marcos` vazio/nulo cai no template vivo, **inclusive célula de folga**. `app/escala/models.py:1117`, `app/escala/models.py:1124`, `app/escala/models.py:1133`. |
| EC.pausas_do_dia / intervalo_do_dia | Leem DNA; diferente do anterior, célula existente sem marcos devolve nenhuma pausa. `app/escala/models.py:1163`, `app/escala/models.py:1182`, `app/escala/models.py:1202`. |
| gerar_celulas_periodo | Pula origem editada antes de qualquer atualização. **Sobrevive.** `app/ponto/portas/celula.py:638`. |
| regenerar_celulas_vinculo | Atualiza existentes somente `origem=='gerada'`, inclusive ao desligar previsão fora da vigência. **Sobrevive.** `app/ponto/portas/celula.py:232`, `app/ponto/portas/celula.py:257`. |
| reverter_regeneracao_dia | Recusa célula não gerada. `app/ponto/portas/celula.py:558`. |
| Fechamento | Carrega todas células do envelope e alimenta motor; previsão passa por EC.eh_dia_trabalho; identity map cobre marcos do motor. `app/ponto/services/fechamento.py:595`, `app/ponto/services/fechamento.py:722`, `app/ponto/services/fechamento.py:741`, `app/ponto/services/fechamento.py:269`. |
| Export | Grade delega a grade_da_celula; esta lê célula+ata, com preferência ao `ata.tipo_dia` e `ata.minutos_previstos` já lavrados. `app/folha/export.py:335`, `app/escala/services/leitor_celula.py:350`, `app/escala/services/leitor_celula.py:380`. Retenção lê `veredito/protestos`, sem filtrar origem. `app/folha/export.py:1206`, `app/folha/export.py:1233`, `app/folha/export.py:1245`. |

**Portanto sobreviver ao gerador não equivale a re-lavrar automaticamente ata/dinheiro.** Export prefere ata e a impressão hoje não vê trabalha; evidências abaixo. `app/escala/services/leitor_celula.py:380`, `app/ponto/services/cartorio.py:443`, `app/ponto/services/cartorio.py:1358`.

Q7 — Conexos importantes:

| Campo/família | Escritor/leitor e consequência |
|---|---|
| `trabalha` fora da impressão | Dívida declarada no catálogo: `app/escala/tests/test_d3_guarda_da_linha_celula.py:43`, :68-72. Hash aceita DNA, não trabalha, e codifica somente `dna_versao` + `marcos`: `app/ponto/services/cartorio.py:443`, :456. Chamada passa `cel.dna`; impressão igual mantém `_em_dia` e pode pular julgamento: `app/ponto/services/cartorio.py:1348`, :1358. Editar apenas trabalha não garante despertar. |
| `editada_em` como freshness | Leitura em `fato_do_dia_mudou`: `app/ponto/services/cartorio.py:365`; lote de freshness das perguntas inclui `editada_em` junto de outros quatro relógios: `app/chamados/veredito_lavrado.py:151`; reabertura das janelas de fase 12x36 agrega Max editada_em e compara lavra: `app/escala/services/fase_12x36.py:188`, :192. Sem escritor humano atual, relógio é preparação já existente, não prova de edição em produção. `app/escala/tests/test_d3_guarda_da_linha_celula.py:50`. |
| outbox da célula | Só veredito/ata acordam `salvar_e_emitir`; `trabalha/dna/origem/editada_*` não estão em CAMPOS_QUE_ACORDAM. `app/ponto/portas/celula.py:663`, :680. |
| ata/veredito/status | Campos de saída lavrados pela mesma porta e usados por export/status; edição de previsão não deve ser confundida com decisão admin. `app/escala/models.py:1749`, :1774, :1810; `app/ponto/portas/celula.py:815`. |
| guardas de folha | Regeneração por vínculo verifica competências lavradas salvo motivo `apesar_da_lavra`; guarda nasce antes do atomic. `app/ponto/portas/celula.py:202`, :244. Não há como assegurar guarda equivalente para edição humana ausente [nao verificado]. |
| vigias | Stale só lê `origem='gerada'`: `app/ponto/management/commands/tripwire_celulas.py:21`. Contra template só geradas: `app/escala/management/commands/celulas_contra_o_template.py:29`. Já folga com marcos conta qualquer origem: `app/escala/management/commands/celulas_contra_o_template.py:43`. |
| DNA completo | Vocabulário de marcos/segundo intervalo é da entidade: `app/escala/models.py:1652`, :1659; `montar_lampadas` encadeia meia-noite e duas pausas: `app/ponto/management/commands/gerar_celulas.py:36`, :55, :60. Um override com marcos incompletos pode deixar consumidores lendo template/sem pausa conforme os contratos distintos acima. |

Nenhum arquivo foi alterado; testes e banco não foram executados. As conclusões são leitura da cópia e dos selos existentes, não resultado de execução [nao verificado]. [nao verificado]

## 06/11 09:16 · sessao 65f096009142 · - · - · origem=Ronald (tmux)

Q3 — conclusões condicionais, pelo código desta cópia. Não há batidas, pares concretos ou valores de folha no dado fornecido; valores financeiros do par são [nao verificado].

| Rubrica | Dia CEDIDO (`trabalha=False`, sem trabalho) | Dia ASSUMIDO (`trabalha=True`, com marcos e trabalho real) | Fonte e decisão |
|---|---|---|---|
| Adicional noturno | Sem período real, não nasce AN; a edição não fabrica trabalho. | Período noturno continua pagando AN normal; mudar de folga para trabalho não é requisito para ter AN, pois a soma já inclui `periodos + periodos_ft`. | `app/ponto/motor_calculo_v2.py:321`; cálculo efetivo por par/intervalos em `:983-987`, escrita única `:994-1005`; entrada dos marcos do vínculo em `:2290-2291` e janela em `:1532-1540`. Fechamento grava `resultado.horas_noturnas` em `app/ponto/services/fechamento.py:909`. |
| Intrajornada | Sem período real, não nasce indenização. | Continua apurando intervalo real versus mínimo; edição não garante valor positivo: depende da duração/pausa. | 12x36: `app/ponto/motor_calculo_v2.py:2766-2784`; comercial: `:3226-3242`; mínimo lê previsto do dia e cadastro/perfil em `:1824-1831`; cadastro inicial vem do template/perfil em `:831-864`. Previsto lê célula/marcos: `app/escala/utils.py:2287-2290`. Fechamento `app/ponto/services/fechamento.py:988`. |
| HE100 por folga/DSR trabalhado | Sem trabalho, não há período para pagar. | `trabalha=True` retira a classificação de folga: vai a `periodos`, não `periodos_ft`; assim deixa de alimentar `horas_folga_trabalhada`. | Juízo lê `EC.eh_dia_trabalho`: `app/ponto/motor_calculo_v2.py:2447-2448`; partição em `:2468-2494`. Fechamento paga folga somente dos `periodos_ft` com escala certa: `app/ponto/services/fechamento.py:924-950`. Template semanal só é fallback sem vínculo que responda: motor `:2449-2453`; previsão injetada prevalece no domingo em `:950-954`. |
| DSR: semanas e reflexo | Dia sai dos obrigatórios porque o cursor do fechamento só inclui `eh_dia_trabalho=True`; logo a ausência nesse dia não vira falta injustificada para DSR. | Entra no previsto e, havendo período normal, entra nas datas trabalhadas; semana e reflexo dependem dos demais dias e extras. | Previsto vindo da célula: `app/ponto/services/fechamento.py:720-731`; `EC` devolve `cel.trabalha` em `app/escala/models.py:1094-1096`. DSR base lê `datas_previstas_trabalho`, períodos e justificativas: `app/ponto/motor_calculo_v2.py:2576-2611`; reflexo `:2636-2653`. Comercial refaz semanas/reflexo em `:3441-3479`. Fechamento grava `app/ponto/services/fechamento.py:1085-1087`. |
| Furo/falta apurada | Dia fora dos obrigatórios deixa de ser `resultado.datas_falta`; isso não é desconto decidido. | Com período normal não é furo; sem trabalho e sem abono, sendo previsto, pode virar furo. | `app/ponto/motor_calculo_v2.py:2595-2619`; teto temporal antes da lavra em `app/ponto/services/fechamento.py:1099-1111`; `DiaPago.furo_apurado` em `app/ponto/services/dia_pago.py:343-345`. O campo não é rubrica/dinheiro: `:150-158`. |
| Falta/desconto já decidido | **Célula sozinha não apaga uma Ausencia DESCONTA aprovada preexistente.** | A edição tampouco cancela esse registro. | `horas_falta` é `minutos_de_falta_por_dia` em `app/ponto/services/fechamento.py:864-866`, gravado em `:987`; essa autoridade lê Ausencia aprovada e catálogo, não célula: `app/folha/export.py:87-92`, `:138-143`. TXT usa `datas_falta` em `:732-736`, cuja fonte é o catálogo DESCONTA em `:179-181`. |
| Atraso/antecipada | Previsto zero zera teto da pontualidade; nenhum período normal também impede cobrar nesse dia. | **A cópia já julga comercial e 12x36 com escala pelos marcos da célula**, portanto atrasos reais em relação ao novo marco podem ser cobrados conforme as regras vigentes. | Teto zero para previsto zero: `app/ponto/motor_calculo_v2.py:1040-1046`; só percorre `resultado.periodos`: `:1078-1084`. Leitor dos marcos do vínculo `:2048-2055`; comercial chama `:3293`; 12x36 com escala chama `:3533`. |

**Limite da conclusão “sem HE100”:** a célula `True` basta para retirar **HE100 por folga**, mas não suprime HE100 de feriado ou de excedente normal acima de duas horas: feriado é outra fonte (`feriado_em_dobra` + calendário), em `app/ponto/motor_calculo_v2.py:934`; excedente 12x36 em `:2847-2854`, comercial em `:3392-3398`. Portanto não afirmar “HE100 global sempre zero” para todo turno só por editar a célula. Esses caminhos existem também no calculador, em `app/ponto/calculador/regras.py:779-815`.

**Nenhuma das rubricas-alvo exige necessariamente modificar `motor_calculo_v2.py` para consumir o override do par:** o ponto exato já lê o vínculo/célula em `app/ponto/motor_calculo_v2.py:2448` e separa folga/trabalho em `:2468-2494`; os marcos vêm do vínculo em `:2290-2291`. AN/intra já usam os períodos reais com esses marcos/janelas (`:2759-2792`, `:3211-3247`). Remover uma falta DESCONTA já decidida exige tratar a decisão em Ausencia, cuja leitura financeira não passa pela célula (`app/folha/export.py:138-143`, `:179-181`), e não é motivo demonstrado para alterar motor. Prova executada do par concreto: [nao verificado].

**S5b do anexo versus estado da cópia:** a cópia registra obra posterior O271 que ligou pontualidade comercial, e o código efetivamente a chama; a regra “calculador não cobra o que o sistema não cobra” permanece, porém comercial saiu dos isentos (`app/ponto/calculador/regras.py:455-472`, `:491-502`; `app/ponto/motor_calculo_v2.py:3293`). Não usar o número histórico do anexo para afirmar que o comercial atual continua `NAO_DECIDE`.

**DSR dinheiro no TXT:** o export não emite evento DSR; documenta que o Domínio calcula reflexo a partir das HE e AN, e a lista devolvida contém essas rubricas sem DSR (`app/folha/export.py:439-460`). Efeito efetivamente pago pelo Domínio no caso: [nao verificado].

**Motor, calculador e lavratura — não confundir as versões:**

- Fechamento mensal recebe AN/intra/HE do motor (`app/ponto/services/fechamento.py:909-915`, `:988`) e DSR do motor (`:1085-1087`), sendo salvo em `:1127-1128`.
- `DiaPago versao='motor'` recebe os mesmos períodos, partições de folga, faltas decididas e reflexo em `app/ponto/services/fechamento.py:1185-1211`; a distribuição soma AN/intra/HE dos períodos em `app/ponto/services/dia_pago.py:173-186`, falta em `:272-275`, DSR numa linha de ajuste em `:346-356`.
- A partir de 10/2026 há uma segunda **versão** `oraculo`, pela mesma função `lavrar`, com valores vindos do calculador (`app/ponto/services/fechamento.py:1249-1278`). Só nessa chamada `rubricas=` sobrepõe campos decididos em `app/ponto/services/dia_pago.py:310-328`; não reescreve o FechamentoMensal nem a versão `motor` (delete é filtrado por `versao`, `:357-358`).
- Calculador também consulta `EC.eh_dia_trabalho` (`app/ponto/calculador/alimentacao.py:434-445`), recebe previsto/célula, intervalo cadastrado, calendário e escala certa em `:514-532`; folga sai pelo ramo `app/ponto/calculador/regras.py:650-704`; AN normal por `:844-849`. Os leitores de export filtram `versao='motor'` em `app/folha/porta_export.py:284`, `:589`, `:651`.

**Q7 conexões adicionais vistas:**

- `datas_justificadas` contém todas as Justificativas aprovadas com `data_turno`, sem filtro de motivo, e também Ausencias cujo efeito é ABONA; SUPRIME remove o dia do previsto (`app/ponto/services/fechamento.py:651-655`, `:684-692`). Manter um pedido `troca_de_plantao` como abono aprovado pode continuar interferindo por esse caminho enquanto não se mudar a semântica do pedido (`:688-690`).
- Ausencia aprovada que explica dia inteiro zera atraso/antecipada na última camada do teto, sem apagar trabalho; essa alimentação vem do fechamento (`app/ponto/services/fechamento.py:682-683`, `:742`; `app/ponto/motor_calculo_v2.py:1085-1105`).
- A edição de marcos não altera a classe do motor por si: `get_motor` escolhe por `tipo_base`/ciclo/geometria do TipoEscala (`app/ponto/motor_calculo_v2.py:3623-3668`), e a política de feriado pelo ciclo em `:3669-3670`.
- Marcação noturna pós-5h/hora reduzida é política de ciclo/CCT, não campo `trabalha` (`app/ponto/motor_calculo_v2.py:758-769`); o mínimo intra também lê cadastro/perfil (`:831-864`, `:1824-1831`). O override é soberano para previsto/marcos, não substitui todas as fontes de dinheiro.

## 06/11 09:15 · sessao 3a25d6a67f22 · - · - · origem=Ronald (tmux)

Referências para os campos das entradas:

| Model | Campos e definições |
|---|---|
| `TipoEscala` | `tipo_ciclo` app/escala/models.py:117; valores `12x36` app/escala/models.py:26 e `6x1` app/escala/models.py:30; `hora_inicio` app/escala/models.py:50; `hora_fim` app/escala/models.py:51; `hora_inicio_intervalo` app/escala/models.py:52; `hora_fim_intervalo` app/escala/models.py:53 |
| `EscalaColaborador` | `colaborador` app/escala/models.py:872; `posto` app/escala/models.py:873; `tipo_escala` app/escala/models.py:874; `data_inicio` app/escala/models.py:875; `data_fim` app/escala/models.py:876; `ativa` app/escala/models.py:877 |
| `CelulaDia` | `colaborador` app/escala/models.py:1681; `data` app/escala/models.py:1685; `trabalha` app/escala/models.py:1686 (False significa folga em app/escala/models.py:1688); `origem` app/escala/models.py:1691; `escala_geradora` app/escala/models.py:1692; `dna` app/escala/models.py:1697; `editada_em` app/escala/models.py:1732; `editada_por` app/escala/models.py:1733; unicidade colaborador/data app/escala/models.py:1833 |
| `TipoAusencia` | `codigo` app/ponto/models.py:577; `efeito` app/ponto/models.py:581; `exige_documento` app/ponto/models.py:583 |
| `Ausencia` | `colaborador` app/ponto/models.py:743; `tipo` app/ponto/models.py:749; `data` app/ponto/models.py:750; `data_fim` app/ponto/models.py:751; `minutos` app/ponto/models.py:755; `hora_ini` app/ponto/models.py:768; `hora_fim` app/ponto/models.py:770; `hora_extra_100` app/ponto/models.py:772; `documento` app/ponto/models.py:777; `documento_referencia` app/ponto/models.py:791; `status` app/ponto/models.py:799; `registrado_por` app/ponto/models.py:801; `aprovado_por` app/ponto/models.py:805; `aprovada_em` app/ponto/models.py:810 |
| `Batida` | `colaborador` app/ponto/models.py:22; `timestamp` app/ponto/models.py:23; `tipo` app/ponto/models.py:24; `is_intervalo` app/ponto/models.py:26; `escala` app/ponto/models.py:29 |
| `LogAuditoria` | `usuario` app/core/models.py:295; `acao` app/core/models.py:296; `modelo` app/core/models.py:297; `objeto_id` app/core/models.py:298; `descricao` app/core/models.py:299; `valor_antes` app/core/models.py:300; `valor_depois` app/core/models.py:301 |
| `ExportacaoDominio` | `empresa` app/folha/models.py:77; `mes` app/folha/models.py:78; `ano` app/folha/models.py:79; `gerado_em` app/folha/models.py:80; `conteudo` app/folha/models.py:86; `invalidada_em` app/folha/models.py:96 |
| `PeriodoFechado` | `empresa` app/ponto/models.py:916; `mes` app/ponto/models.py:920; `ano` app/ponto/models.py:921; `ativo` app/ponto/models.py:933 |

Regras pertinentes ao vínculo:

- Autoridade do vínculo do dia é `CelulaDia.escala_geradora`, não o vínculo ativo ou mais recente: app/escala/alimentacao.py:231.
- Corte E1 literal: geradora só vale dentro de `data_inicio..data_fim`; fora, retorna `None` e o dia é de cadastro; vínculo hoje inativo continua valendo se cobria o dia: app/escala/alimentacao.py:272.
- Sem geradora, o vínculo desativado e sem `data_fim` não cobre dia algum: app/escala/alimentacao.py:209.
- Célula editada é imutável **pelo gerador**, sem regra nesse trecho sobre sobrescrita por outro ato humano: app/escala/models.py:1640.

Resolver dia:

- Função `resolver_dia_oferecido(dia, hoje)` app/colaboradores/services/calendario.py:97.
- Entrada `dia` é um item de `contexto_calendario()['dias']`, conforme docstring app/colaboradores/services/calendario.py:98.
- Predicado documentado: último recurso + passado + status que pede decisão; permissão não integra o predicado: app/colaboradores/services/calendario.py:100.
- Campos do dict usados: `pode_ultimo_recurso`, `data`, `status` app/colaboradores/services/calendario.py:103. Status previstos `falta`, `irregular`, `aberto`, `sem_certidao`: app/colaboradores/services/calendario.py:94.

Campos específicos de par/cedente/substituto/identidade da troca/segundo anexo/estado `confirmada`: [nao verificado]. O modelo existente documentado possui `Ausencia.documento` e `documento_referencia`, com a última referência destinada ao primeiro registro de um intervalo, não a um par de troca: app/ponto/models.py:777 e app/ponto/models.py:791.

A catraca atual declara `editada_em` e `editada_por` sem escritor de produção: app/escala/tests/test_d3_guarda_da_linha_celula.py:50. Isso é evidência da cópia, não regra para esperado do O10.

## 06/11 09:15 · sessao 5df745df4184 · - · - · origem=Ronald (tmux)

Fontes de regras e campos para bordas/adversariais:

- **O10 completo:** `app/docs/BACKLOG.md:1224` e `app/docs/CORTES.md:187`: par, cedido folga sem desconto, assumido normal com AN/intra sem HE100, pedido só vale confirmado pelo admin com cartas dos dois lados, `trocas_sem_par`, conversão legado por DRY→aval.
- **S5b literal do anexo:** `app/docs/BACKLOG.md:1248`: não cria desconto novo; classe que não julga pontualidade não passa a cobrar.
- **Idempotência normativa:** `CLAUDE.md:175`: “2x = mesmo estado, 1 linha de trilha, 0 duplicata”.
- **Competência pela data:** `app/docs/CORTES.md:216`: data do fato, 21–20, nunca mês civil. `app/ponto/janelas.py:10`–`:14` declara corte 21 = 21 do mês anterior até 20 do mês de competência.
- **Campo do corte:** `Empresa.dia_inicio_competencia` em `app/colaboradores/models.py:46`.
- **Meia-noite:** `app/escala/servico_jornada.py:61`–`:64`: marcos ancorados no dia e encadeados; `:96`–`:103`: fim≤início cruza, inclusive duração 24h. Isso define geometria; não define rateio O10 entre competências.
- **Exportada:** L-092, `app/docs/LEIS.md:125`: gravado não muda; diferença vira Pauta DP com os dois números. Exceção exige `!` e porta própria, `CLAUDE.md:403`–`:409`.
- **Célula já editada:** `app/escala/models.py:1638`–`:1643` só assegura que o **gerador** não reescreve `origem='editada'`; não decide conflito com outra edição humana.
- **Campos da célula:** colaborador `app/escala/models.py:1681`; data `:1685`; trabalha `:1686`; origem `:1691`; escala_geradora `:1692`; dna `:1697`; editada_em `:1732`; editada_por `:1733`.
- **Dia já trabalhado:** `Batida.colaborador` `app/ponto/models.py:22`; timestamp `:23`; tipo `:24`.
- **FechamentoMensal:** colaborador `app/ponto/models.py:405`; mes `:408`; ano `:409`; status `:410` (opção exportado `:402`); horas_noturnas `:413`; horas_extras_100 `:417`; horas_folga_trabalhada `:423`; horas_atraso `:425`; horas_saida_antecipada `:426`; horas_falta `:427`; horas_intra_indenizada `:428`.

Ressalva documental: a fotografia S5b fornecida diz que `MotorComercial` não julga; há corte posterior O271 em `app/docs/BACKLOG.md:1512`. `app/ponto/calculador/regras.py:466`–`:472` documenta que comercial passou a julgar. Não usar comportamento atual para substituir o esperado do recorte solicitado.

Não encontrei regra específica O10 escrita para aceitar/recusar dia cedido já trabalhado, substituir edição humana existente, confirmar colega sem vínculo no dia, ordem/vencedor de confirmações concorrentes distintas, status HTTP/texto exato de recusa, chave formal de identidade do par ou rateio de AN/intra entre competências. Nessas partes, esperado **“regra nao decide”**. Atomicidade do par vem do aval FATIA B fornecido; não encontrei extensão disso que determine resposta individual a cada requisição concorrente.

## 06/11 09:15 · sessao 2bee5edba262 · - · - · origem=Ronald (tmux)

- **Contrato central:** `app/docs/CORTES.md:187` e `app/docs/BACKLOG.md:1224`: “troca e ATO com par”; “dia cedido = folga trocada sem desconto”; “dia assumido = dia normal com AN/intra, sem HE100”; “override por data com trilha”; pedido só vale confirmado pelo admin com carta do cedente e carta do substituto. Pode nascer de qualquer lado; “troca confirmada sem 2 anexos = vermelho”; contador `trocas_sem_par`.
- **Idempotência observável:** `app/docs/ESTRUTURAL.md:22`–`:25`: “chamar 2x com a mesma entrada = mesmo estado, 1 linha de trilha, 0 chamado/pergunta duplicada”. Também `CLAUDE.md:175`.
- **S5b:** `app/docs/PROMPTS.md:313` e `app/docs/BACKLOG.md:1248`: “a troca da S5b NAO cria desconto novo”; classes `MotorComercial`, `Motor12x36`, `MotorIntermitente` permanecem `NAO_DECIDE` no calculador. `app/docs/PROMPTS.md:341`: classe que não julga pontualidade não passa a cobrar; todo outro campo de todo colaborador dá zero, L-082(b).
- **Exportada:** o literal do aval fornecido desta obra proíbe “tocar exportada sem !”. Não converter em expectativa de HTTP 403 ou recusa de negócio: esse comportamento não foi especificado. Há tensão histórica na cópia: `app/docs/LEIS.md:150` (L-113) revoga a recusa geral de recálculo em exportada; `CLAUDE.md:403` preserva a restrição de alteração por caminho genérico, e `CLAUDE.md:414` afirma correção provada em qualquer competência. O caso desta obra deve ancorar no aval específico e reconhecer que resposta/status não são definidos.
- **Legado:** `app/docs/BACKLOG.md:1224` corrige censo para 16 (12 aprovados, dois rejeitados, dois aguardando), todos SOLO, `data_fim` nulo, tipo `efeito='abona'`; conversão exige DRY → aval. Esses números são evidência documental, não nova leitura de banco.
- **Atomicidade:** requisito específico “duas edições [...] em atomic”, usuário/antes/depois/motivo e idempotência da segunda confirmação estão no aval fornecido; não encontrei a versão nova O10-TROCA-DE-PLANTAO-ENTRA gravada nos docs da cópia.
- **Lacunas localizadas:** contrato não define aprovação/recusa para dia já trabalhado, célula anteriormente editada, colega sem vínculo ativo, troca de vínculo entre as datas, conflitos entre pares distintos, datas vazias/invertidas/iguais, janela máxima ou semântica exata de confirmação concorrente. A atomicidade sustenta “nenhum meio par confirmado visível”; idempotência sustenta ausência de duplicação do mesmo ato, mas não escolhe código/status de cada concorrente.
- Não há `AGENTS.md` nesta cópia na busca de arquivos; nenhuma escrita efetuada.

## 06/11 09:13 · sessao edfebd45fd2d · - · - · origem=Ronald (tmux)

Censo runtime completo enviado nas mensagens anteriores, agrupado por arquivo e linha; as escritas persistentes ficam nestes pontos:

| Alvo | Escritores |
|---|---|
| `_adotar_vivas_na_abertura` | Definição `app/chamados/models.py:434`; chamada por `abrir` em `:1027` e `reabrir` em `:1496`. |
| Função removida | Restam lápides em `app/chamados/models.py:517`, `app/chamados/services/fio.py:899` e referência desatualizada em `app/docs/RELATO.md:13`. |
| `DisputaSupervisao.fechada_em` | `fechar`: `app/chamados/models.py:1955`; `reabrir_admin`: `:2157`; `reabrir_sistema`: `:2181`. |
| `DisputaSupervisao.resolucao` | `fechar`: `app/chamados/models.py:1957`; `reabrir_admin`: `:2159`; `reabrir_sistema`: `:2183`; porta adicional `limpar_resolucao_residual`: `:2267`. |
| `PerguntaDisputa.chamado_destino` | Nascimento em `app/chamados/models.py:2571`; reaponte de existente em `:2544`, persistido em `:2568`; porta `reapontar_resolvedora` em `app/chamados/services/fio.py:845`, persistida em `:847`. |

`reabrir_por_colab` lê e recusa disputa fechada; não escreve `fechada_em`. (`app/chamados/models.py:2232`)

As atribuições `p.chamado_destino` em `app/chamados/services/fio.py:773` e `:777` simulam o destino para julgamento e restauram a instância; a atribuição em `app/chamados/models.py:350` atualiza a referência em memória. As persistências do campo estão nos pontos da tabela. (`app/chamados/services/fio.py:772`; `app/chamados/models.py:349`)

Os leitores diretos de `resolucao` são a idempotência do fecho, a limpeza residual, a cópia da resolução para ocorrência no fio, o fallback do materializador e o snapshot de reversão do comando. (`app/chamados/models.py:1915`; `app/chamados/models.py:2264`; `app/chamados/models.py:2266`; `app/chamados/signals.py:45`; `app/chamados/services/materializacao.py:1253`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:132`)

As migrações relevantes são de esquema: `fechada_em` e `resolucao` em `app/chamados/migrations/0013_disputasupervisao.py:24` e `:25`; `chamado_destino` em `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:18`. Não são segundos escritores de dados. (`app/chamados/migrations/0013_disputasupervisao.py:24`; `app/chamados/migrations/0016_s84_pergunta_chamado_destino.py:18`)

O índice lexical de testes é volumoso: 74 arquivos mencionam `fechada_em`, 25 `resolucao`, 191 `chamado_destino`; esses números incluem strings de selos e parâmetros de filtros, portanto não equivalem a leitores semânticos. [nao verificado] Os índices mais úteis são os selos de escritores e os testes que preparam diretamente a disputa fechada. (`app/chamados/tests/test_selo_resolucao_um_escritor.py:44`; `app/chamados/tests/test_c7_red_minimos.py:39`; `app/core/tests/test_contract_escritores_estado.py:61`; `app/chamados/tests/test_c20_fio_mudo.py:245`; `app/chamados/tests/test_selo_i8_fio_solto.py:201`; `app/chamados/tests/test_hx_inv_fio.py:78`)

Achado principal adicional: **o alarme F2 continua tratando como violação um estado que a nova lei aceita**. Ele seleciona perguntas vivas sem materialização/validação, em disputa fechada e colaborador ativo, e termina com `ALARME`/exit 2; o cron continua registrado. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1761`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1853`; `app/config/crons.py:542`) O teste antigo exige esse alarme, enquanto o contador escolhido pela lei diz expressamente “dono ADMIN” e “NAO alarma”. (`app/chamados/tests/test_hx_inv_fio.py:75`; `app/chamados/services/vigia_fio.py:740`)

Não encontrei import vivo da função removida; a documentação do topo ainda descreve o callback e a decisão como pendente. (`app/chamados/models.py:517`; `app/chamados/services/fio.py:899`; `app/docs/RELATO.md:13`) MAPA e docstrings conservam a regra ampla de ressuscitação com reabertura, que ainda corresponde a caminhos existentes, mas carece de ressalva para a adoção muda. (`app/docs/MAPA/chamado.md:89`; `app/chamados/services/relavra_via.py:69`; `app/chamados/models.py:1625`; `app/chamados/services/relavra_via.py:244`)

## 06/11 09:12 · sessao 0b779a516451 · - · - · origem=Ronald (tmux)

Q3 — caminhos e alcance:

- **`reapontar_resolvedora` não reabre nem envia push diretamente**: escreve destino, lavra e salva a pergunta; o `post_save` agenda fechamento de beco, cujo classificador exclui disputa FECHADA. Alcançado diretamente pela adoção. (`app/chamados/services/fio.py:845-849`; `app/chamados/models.py:502`; `app/chamados/signals.py:191-202`; `app/chamados/services/becos.py:27-28`)
- **`migrar_resolvedoras_vivas` também não reabre diretamente**: usa reapontamento e retratação; é chamado quando `Chamado.reabrir` encontra irmão vivo. Esse ramo retorna antes da ressuscitação e da adoção. (`app/chamados/services/fio.py:944-967`; `app/chamados/models.py:1465-1480`)
- **Continua existindo reabertura automática de FECHADA em `ressuscitar_perguntas`**: limpa carimbos em lote e chama `d.reabrir_sistema` para disputas fechadas. Só alcança pergunta materializada, sem texto de resposta, que passe pelos guardas do marco e C6. (`app/chamados/models.py:1528-1552`; `app/chamados/models.py:1593-1626`)
- **Esse caminho é alcançável desde a abertura de `batida_ausente`**, condicional aos guardas: `_abrir_chamado` procura por módulo/data **sem filtro de status**; `abrir` pode achar encerrado, chamar `_renascer_por_premissa_viva`, que chama `reabrir`; este ressuscita **antes** de adotar. Assim, não é correto declarar que todo fluxo `abrir` passou a preservar FECHADA. (`app/ponto/management/commands/detectar_ausencias.py:693-713`; `app/chamados/models.py:974-977`; `app/chamados/models.py:1058-1074`; `app/chamados/models.py:1495-1496`)
- **O cartório também chama ressuscitação**, para cobrança viva sem fio vivo e sem texto de resposta; a vigia de hora entra no cartório. Não é consequência obrigatória do simples reapontamento; depende da seleção e dos guardas. (`app/ponto/services/cartorio.py:1591-1622`; `app/ponto/services/vigia_de_hora.py:131`)
- **Push posterior à reabertura**: o cron diário seleciona somente disputas abertas e chama `push_colaborador`, sem consultar C6 ou existência de pergunta pendente. Está agendado às 08h. Logo a reabertura sobrevivente habilita outro cano posterior ao colaborador. (`app/ponto/management/commands/alertar_disputas_pendentes.py:33-64`; `app/config/crons.py:745`)
- **Fábrica depois da abertura é outro caminho de push**: `_abrir_chamado` coleta órfãos e chama a fábrica com `notificar=True` padrão; esta chega ao push se a disputa não puder ser fechada. Uma pergunta existente pode continuar na sua disputa FECHADA, porque o gate reaponta o destino sem trocar `disputa`; havendo outras perguntas vivas na disputa aberta, a fábrica pode notificar o colaborador. Não afirmar push incondicional: disputa vazia fecha antes do push. (`app/ponto/management/commands/detectar_ausencias.py:790-801`; `app/chamados/services/disputa_emissao.py:876`; `app/chamados/services/disputa_emissao.py:925-937`; `app/chamados/services/disputa_emissao.py:1112-1124`; `app/chamados/models.py:2525-2569`)
- O caso **passado com marcos nomeados** atravessa o filtro da fábrica mesmo havendo pergunta não materializada: `cobra_marcos_da_ata(ch)` constitui alternativa explícita ao filtro de inexistência de pergunta. Isso não é exercitado pelo R08f do dia atual. (`app/chamados/services/disputa_emissao.py:658-680`; `app/chamados/services/disputa_emissao.py:925-930`; `app/chamados/tests/test_c20_fio_mudo.py:241-249`)

Outras portas que sobrevivem, sem chamada direta pela adoção:

| Caminho | Efeito e alcance |
|---|---|
| `desfazer_carimbo_sem_lastro` | Default permite FECHADA e chama `reabrir_sistema`; o evento do cartório passa `reabrir_fechada=False`, portanto esse uso protege FECHADA. `perguntar` conserva o default, porém sua seleção de motivos ainda condiciona o alcance. (`app/chamados/services/relavra_via.py:185`; `app/chamados/services/relavra_via.py:221-245`; `app/chamados/services/fabrica_da_celula.py:136-138`; `app/chamados/services/fabrica_da_celula.py:164-176`) |
| Validação recusada | Default reabre FECHADA; o fio do admin passa `sem_reabrir=True` quando encontra a disputa fechada. Não chamado pela adoção. (`app/chamados/services/validacao.py:155`; `app/chamados/services/validacao.py:197-200`; `app/chamados/services/fio.py:596-605`) |
| Ausência rejeitada | Signal de Ausência chama devolução das absorvidas; esta reabre as disputas por sistema. Não chamado diretamente pela adoção. (`app/ponto/signals.py:261-262`; `app/chamados/services/materializacao.py:638-665`) |
| `reabrir_orfas_enterradas --apply` | Reabre pergunta fantasma e disputa FECHADA; comando expressamente de uma vez, não cron regular. Não chamado pela adoção. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:147-162`; `app/config/crons.py:785-788`) |
| Reabertura administrativa | Reabre disputa, cria pergunta e envia FCM ao colaborador. Depende de ação admin, motivo e justificativa; não chamado pela adoção. (`app/chamados/services/acoes_disputa.py:242-264`; `app/chamados/services/acoes_disputa.py:283-289`) |
| Desarquivar pela tranca | Chama `Chamado.reabrir` e posteriormente ressuscita de novo após levantar o véu. Não chamado pela adoção. (`app/chamados/views_arquivar.py:65-86`) |
| Reabrir competência | Desfaz perguntas pela via de competência e chama `Chamado.reabrir(humano=True)`; pode alcançar sua ressuscitação, conforme guardas. Não chamado pela adoção. (`app/chamados/services/competencia_trancada.py:250-265`) |
| Reabrir por colaborador | **Não reabre FECHADA**: retorna `disputa_fechada`; só devolve questionário de disputa já aberta. (`app/chamados/models.py:2232-2242`) |

Não transformar todas essas portas em defeitos da obra automaticamente: parte tem atos próprios e leis específicas; a violação demonstrável no alcance da obra é o ramo de renascimento/ressuscitação que o fluxo de emissão pode executar. (`app/chamados/models.py:974-976`; `app/chamados/models.py:1495-1496`; `app/chamados/models.py:1625-1626`)

Q6:

- Não encontrei segundo escritor executável de **`fechada_em` em produção fora das portas**: fechar escreve timestamp e salva; reabrir_admin e reabrir_sistema limpam e salvam. A reabertura que sobrou usa a porta existente, portanto é problema de **chamador/comportamento**, não de segundo escritor. (`app/chamados/models.py:1955-1959`; `app/chamados/models.py:2157-2169`; `app/chamados/models.py:2181-2185`; `app/chamados/models.py:1626`)
- `resolucao` tem uma porta adicional legítima, `limpar_resolucao_residual`, limitada a disputa ABERTA e sem escrita em `fechada_em`. (`app/chamados/models.py:2264-2268`)
- O teste novo escreve `fechada_em/resolucao` por `QuerySet.update` como fixture; isso contorna as portas e os signals, mas não constitui escritor de produção. (`app/chamados/tests/test_c20_fio_mudo.py:245`)
- Nenhuma leitura de relógio foi acrescentada ao código de produção pelo delta fornecido; a alteração remove callback/função e troca documentação. O novo teste fornece instantes por `_ts` e `freeze_time`. (`app/chamados/models.py:516-519`; `app/chamados/services/fio.py:899-902`; `app/chamados/tests/test_c20_fio_mudo.py:243-269`)
- Execução efetiva desses caminhos em produção, quantidade de atingidos e ocorrência real de push: **[nao verificado]**. [nao verificado]

## 06/11 09:11 · sessao 3e2d3d0ddd4c · - · - · origem=Ronald (tmux)

**R08f morde o defeito original, mas não certifica toda a lei.** Ele prepara disputa fechada com resolução preservável, pergunta respondida e viva, executa `vigia_de_hora` com callbacks e exige reapontamento, `(fechada_em, resolucao)` intactos e três mocks sem chamadas; depois consulta montagem e modal do admin. [app/chamados/tests/test_c20_fio_mudo.py:243-279]

**Lacunas concretas e mutantes que passam:**

- **Reabrir somente perguntas não respondidas.** A fixture do R08f força `respondida_em` preenchida; o ramo não respondido não é exercitado. Um mutante `if p.respondida_em is None: p.disputa.reabrir_sistema(...)` sobreviveria a esse teste. [app/chamados/tests/test_c20_fio_mudo.py:247-254] [nao verificado]
- **Reabrir em outro ato.** O teste chama somente o vigia; não chama `reabrir()`, `migrar_resolvedoras_vivas`, `ressuscitar_perguntas`, outro emissor ou cron posterior. O R08r separado testa `reabrir()`, mas não prepara disputa fechada nem verifica fechamento/push. [app/chamados/tests/test_c20_fio_mudo.py:150-171,253-254] [nao verificado]
- **Push pelo wrapper FCM/Web Push padrão pode ficar invisível por fixture sem canal.** A fixture compartilhada cria colaborador sem `usuario` e sem `fcm_token`; ambos aceitam nulo. `push_fcm_colaborador` retorna antes do mock de `push_fcm` quando falta token; `push_colaborador` retorna antes do mock de `_enviar_push_usuario` quando falta usuário. Assim, introduzir chamadas a esses wrappers pode deixar `[[],[],[]]` igual. [app/chamados/tests/test_pergunta_do_dia_em_curso.py:34-35; app/colaboradores/models.py:351,354; app/chamados/fcm_utils.py:104-106; app/chamados/push_utils.py:114-118; app/chamados/tests/test_c20_fio_mudo.py:224-226,262]
- **Mudar o card do admin para o chamado antigo.** Mutante em `montar_chamados_fio`: substituir `alvo = chamado_que_lista(p, _d)` por `alvo = _d.chamado_id`. A disputa continua ligada ao morto da fixture; ele também está no universo do admin. O teste achata todos os cards e exige somente a presença da pergunta no HTML, sem conferir que ela está no card do chamado novo. [app/chamados/services/fio.py:174-176,236-249; app/chamados/tests/test_c20_fio_mudo.py:242-243,266-278]
- **Mostrar Corrigir indevidamente.** A fixture fabrica pergunta `tipo_resposta='escolha'`; o teste proíbe apenas `recusar_pergunta`. A ação `corrigir_resposta` é outro formulário, condicionado a pergunta de hora. Um erro só nesse ramo sobreviveria. [app/chamados/tests/test_c20_fio_mudo.py:65-69,278; app/templates/chamados/partials/modal_fio.html:295-310]
- **Reabrir/push ao clicar Validar.** O teste somente faz GET e procura texto; não envia o POST. Os mocks e a verificação de fechamento já terminaram antes desse GET. Portanto retirar `sem_reabrir=_fechada_na_porta` da delegação ou introduzir push no POST não é detectado. O parâmetro hoje chega à validação e impede `devolver_validacao_recusada` de chamar `reabrir_sistema` quando a porta de materialização recusa uma resposta de hora. [app/chamados/tests/test_c20_fio_mudo.py:253-278; app/chamados/services/fio.py:596-605; app/chamados/services/acoes_disputa.py:19-22; app/chamados/services/validacao.py:197-200,422-427]

**Os três mocks não alcançam todos os canos da árvore.** Há Web Push direto ao colaborador, usando `pywebpush.webpush`, em `processar_alertas_turno.Command._enviar_push` e `processar_alertas_avancados.Command._enviar_push_nivel3`; nenhum passa pelos três alvos. Isso refuta a completude anunciada pela docstring de `_push_gravado`. Não há evidência nestes arquivos de que esses comandos sejam chamados pela abertura do vigia/detector; ligação efetiva entre eles e a adoção é [nao verificado]. [app/chamados/tests/test_c20_fio_mudo.py:217-226; app/ponto/management/commands/processar_alertas_turno.py:247-251,275-288; app/ponto/management/commands/processar_alertas_avancados.py:195-212]

**Precisão sobre aliases:** imports de `push_fcm_colaborador` no topo de outros módulos não furam, por si só, o patch de `push_fcm`: o wrapper resolve esse global no módulo original. O mesmo vale para `push_colaborador` e `_enviar_push_usuario`. O problema demonstrável são os guards anteriores ao mock e os envios diretos, não um alias comprovado desses três alvos. [app/chamados/fcm_utils.py:101-106; app/chamados/push_utils.py:114-118; app/chamados/tests/test_c20_fio_mudo.py:224-226]

**Contador não está órfão nem descreve reabertura antiga:** `viva_em_disputa_fechada` declara dono ADMIN, “sem reabrir”, sem alarme; conta vivas pelo juiz e é consumido pelo comando `vigiar_fio_morto`. [app/chamados/services/vigia_fio.py:737-759; app/chamados/management/commands/vigiar_fio_morto.py:174,225]

**Outro comentário deixado impreciso, anterior ao delta:** a docstring de `devolver_validacao_recusada` afirma incondicionalmente que disputa fechada reabre, mas o código possui a exceção `reabrir=False`, empregada pelo fio do admin. [app/chamados/services/validacao.py:171-174,197-200; app/chamados/services/fio.py:605]

A execução do teste e a sobrevivência experimental desses mutantes não foram verificadas; os resultados acima são análise estática. [nao verificado] [nao verificado]

## 06/11 08:57 · sessao 70a8568b4804 · CXR-NA-FILA · leitura · origem=Code

**CXR-TELA-LE-O-MOTOR está no bloco OBRAS/fila 1**, cuja abertura declara “A FILA 1 VIVA”; sua linha é a 775. Estado: **“registrada — depois da JUIZ-DO-PAR-DUAS-ENTRADAS”**. Dono da obra: **[nao verificado]** — a linha não o declara, e o cabeçalho tem `estado`, `aval` e `nota`, sem coluna `dono`. A dependência já consta como **NO AR em 29/10**. ([BACKLOG.md:714](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:714), [BACKLOG.md:726](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:726), [BACKLOG.md:753](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:753), [BACKLOG.md:775](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:775))

Para fechar, o texto exige levar à tela os dias de `dias_cadastro_x_realidade`, inclusive `l084_pausa`/L-084, mediante **assinatura com contador diário lavrado + cron + serviço**, evitando varrer a frota no request. RELATO, PROMPTS e CORTES confirmam seu registro como obra separada; não registram ali seu fechamento. ([BACKLOG.md:775](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:775), [RELATO.md:1385](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:1385), [PROMPTS.md:1558](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/PROMPTS.md:1558), [CORTES.md:318](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/CORTES.md:318))

**Existe código da tela e da lista**, mas a integração pedida não aparece implementada no corpo de `lista()`: ele monta assinaturas de cadastro e lê a lavra do esmeril. Existe leitura de `dias_cadastro_x_realidade` em `InsumosDoDono.cxr()`, usada para decidir **dono**, o que não equivale a expor todos esses dias na tela. ([views.py:484](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/escala/views.py:484), [cadastro_realidade.py:710](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/escala/services/cadastro_realidade.py:710), [cadastro_realidade.py:723](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/escala/services/cadastro_realidade.py:723), [cadastro_realidade.py:802](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/escala/services/cadastro_realidade.py:802), [e6_oraculo.py:160](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:160))

Os movimentos **185→200** e **18→39** são tratados como dado raspado fornecido; sua conferência em PROD e o vínculo individual dos casos com uma cura permanecem **[nao verificado]**.

Nas definições abaixo, ambas as classes já passaram pelos filtros: dia retido na folha, passado, sem porta alcançável e sem enquadramento nos baldes de lei. ([vigia_fio.py:585](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:585), [vigia_fio.py:642](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:642), [vigia_fio.py:648](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:648), [vigia_fio.py:654](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:654))

| classe | definicao (arquivo:linha) | obra que cobre | mecanismo | estado da obra | o que falta |
|---|---|---|---|---|---|
| **d** | Com célula, sem chamado vivo nem pergunta viva; **colaborador presente na lista CxR, mas dia explicitamente ausente**: `not ps and no_dia == (False, None) and lista.colab_na_lista(cid)`. A classe não determina dono. ([vigia_fio.py:523](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:523), [vigia_fio.py:667](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:667), [vigia_fio.py:677](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:677)) | **CXR-TELA-LE-O-MOTOR cobre a omissão de dias do motor na tela; não demonstra cura dos d/ESTRUTURA.** ([BACKLOG.md:775](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:775), [RELATO.md:41](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:41)) | Se a cura acrescentar o **dia**, com dono CADASTRO/BATIDA, ele poderá sair para `fio_do_admin`; acrescentar apenas a pessoa não basta. **ESTRUTURA não sai por essa regra.** Mantidos os insumos, o juiz já consulta a CxR do motor antes de devolver ESTRUTURA; mudar a leitura da tela não muda esse dono. Se um dia ESTRUTURA passar a constar na lista, pode apenas mudar de `d` para `e/dia_na_lista_dono_estrutura`, ainda sem porta. ([cadastro_realidade.py:955](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/escala/services/cadastro_realidade.py:955), [vigia_fio.py:228](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:228), [e6_oraculo.py:183](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:183), [vigia_fio.py:681](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:681)) | Registrada, depois da JUIZ-DO-PAR. ([BACKLOG.md:775](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:775)) | Implementar a leitura pela assinatura/lavra/cron/serviço descritos. Quantos dos casos raspados pertencem aos dias omitidos do motor: **[nao verificado]**. ([BACKLOG.md:775](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:775)) |
| **e-sem-viva** | `causa='e'`, `detalhe='sem_pergunta_viva'`: com célula, sem chamado vivo e sem pergunta viva, **depois de excluir `d` e o detalhe `dia_na_lista_dono_*`**. Pode haver perguntas antigas já mortas/validadas; não significa “nunca houve pergunta”. ([vigia_fio.py:667](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:667), [vigia_fio.py:677](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:677), [vigia_fio.py:681](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:681), [test_o261e_f5_alcance.py:427](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/tests/test_o261e_f5_alcance.py:427)) | **C20-FIO-MUDO não cobre diretamente esse estado pelo universo declarado**: trata `batida_ausente` **vivo** sem pergunta viva. ([RELATO.md:48](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:48)) | Seu evento atua na abertura do chamado e na recuperação de perdas de pergunta. Já `e-sem-viva` não tem chamado vivo no dia — caso contrário cairia antes em `a`, ou teria porta. O RELATO declara **zero do estoque 494/495 alcançável pelo evento**, mantendo o cron para esse estoque. ([BACKLOG.md:1769](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1769), [vigia_fio.py:664](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:664), [RELATO.md:6670](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:6670)) | **ABERTA, em raia**; código aprovado pelo opus, suíte registrada como verde, pouso esperando lei. ([BACKLOG.md:1769](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1769), [RELATO.md:6674](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:6674)) | Decidir reabrir disputa fechada ao reatar versus apenas oferecer Validar; quatro dos cinco atos continuam sem evento, além das seis vivas sob outro chamado vivo. ([RELATO.md:13](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:13), [RELATO.md:6670](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:6670), [RELATO.md:6671](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:6671)) |
| **e-sem-viva** | Mesmo ramo `e/sem_pergunta_viva`. ([vigia_fio.py:685](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:685)) | **LEI-FIO-MUDO-B: [nao verificado]**; não localizei obra com esse identificador. | Existe uma **alternativa B** na pendência da C20: manter disputa fechada e apresentar pergunta ao admin com Validar. Isso trata pergunta reatada; não demonstra emissão para os casos sem fio nem comprova uma obra chamada LEI-FIO-MUDO-B. ([RELATO.md:13](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:13)) | **[nao verificado]** para esse identificador. | Texto e registro dessa obra: **[nao verificado]**. |
| **e-sem-viva** | Mesmo ramo `e/sem_pergunta_viva`. ([vigia_fio.py:685](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:685)) | **O261 / GRUPO-D-DIA-ACUSA-SEM-FIO** é a cobertura documental para dia acusando sem pergunta e sem chamado; alcance aos 39: **[nao verificado]**. ([BACKLOG.md:1450](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1450), [BACKLOG.md:1105](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1105)) | O261 manda identificar quem acendeu a lâmpada sem emitir e curar o emissor que a lê. GRUPO-D registra encaminhamento de `nunca_bateu` à lista DP e cura separada de `sem_celula`; esta última é **outro detalhe de e**, não `e-sem-viva`. ([BACKLOG.md:1450](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1450), [BACKLOG.md:1105](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1105), [vigia_fio.py:667](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:667)) | O261 registrada como FECHADA em 24/10; GRUPO-D como NO AR em 25/10, com essas curas nomeadas. ([BACKLOG.md:1450](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1450), [BACKLOG.md:1105](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:1105)) | Identificar a origem dos casos atuais e sua correspondência com os recortes curados: **[nao verificado]**. O complemento exige **cura por classe depois da medida**, posteriormente absorvida pela O261-E. ([BACKLOG.md:963](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/BACKLOG.md:963)) |

**Quem escreve e quem lê a classificação:**

| papel | funções e evidência |
|---|---|
| **Deriva/escreve em memória** | `censo_alcance()` atribui `d` e `e/sem_pergunta_viva`, incrementa contadores; sua função interna `_listar()` guarda causa e detalhe por dia. O serviço declara nenhuma escrita no banco. ([vigia_fio.py:545](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:545), [vigia_fio.py:625](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:625), [vigia_fio.py:677](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:677), [vigia_fio.py:685](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:685), [vigia_fio.py:695](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:695)) |
| **Lê/formata/imprime** | `linhas_alcance()` lê contadores de causa e detalhe; `vigiar_fio_morto.Command.handle()` chama o censo e imprime essas linhas. ([vigia_fio.py:716](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:716), [vigiar_fio_morto.py:173](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/management/commands/vigiar_fio_morto.py:173), [vigiar_fio_morto.py:183](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/management/commands/vigiar_fio_morto.py:183)) |
| **Persiste a fotografia** | `Command.handle()` entrega o bloco `alcance` inteiro a `_carimbar()`, que grava JSON e substitui o arquivo. ([vigiar_fio_morto.py:211](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/management/commands/vigiar_fio_morto.py:211), [vigiar_fio_morto.py:223](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/management/commands/vigiar_fio_morto.py:223), [vigiar_fio_morto.py:132](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/management/commands/vigiar_fio_morto.py:132)) |
| **Lê o carimbo, sem expor d/e individualmente** | `bin/fio_estado.py::ler()` carrega o JSON; `_ler_carimbo()` lê `alcance`, mas expõe universo, cegueira e hoje/futuro. ([fio_estado.py:98](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/bin/fio_estado.py:98), [fio_estado.py:142](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/bin/fio_estado.py:142), [fio_estado.py:151](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/bin/fio_estado.py:151)) |
| **Decide dono, separadamente da classe** | `InsumosDoDono.cxr()` lê os dias do motor; `.dono()` chama `dono_da_divergencia()`, que decide CADASTRO/BATIDA/ESTRUTURA. A própria QUEIMA-A distingue “CxR do MOTOR, não a da tela”. ([e6_oraculo.py:154](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:154), [e6_oraculo.py:172](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:172), [e6_oraculo.py:98](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:98), [RELATO.md:41](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:41)) |
| **Produz o cruzamento classe × dono do anexo** | Implementação e equivalência das sondas raspadas: **[nao verificado]**. |

## 06/11 08:54 · sessao 2a5c63b89837 · - · - · origem=Ronald (tmux)

- **CXR está no bloco OBRAS/fila 1**, entre `OBRAS:INICIO` e `OBRAS:FIM`; o comentário declara que o hook lê esse bloco e cobra o primeiro aberto. `app/docs/BACKLOG.md:712`, `app/docs/BACKLOG.md:714`, `app/docs/BACKLOG.md:723`, `app/docs/BACKLOG.md:1800`.
- **Estado:** “registrada — depois da JUIZ-DO-PAR-DUAS-ENTRADAS”. A linha não declara dono; a tabela tem colunas `id/o que/estado/aval/nota/quando`, sem coluna dono. **Dono nominal [nao verificado]**. A dependência consta como NO AR em 29/10. `app/docs/BACKLOG.md:726`, `app/docs/BACKLOG.md:753`, `app/docs/BACKLOG.md:775`.
- **Entrega exigida:** a tela deve ler `dias_cadastro_x_realidade` do motor e mostrar dias `l084_pausa`/L-084, usando assinatura com contador diário lavrado + cron + serviço para evitar frota no request. O corte separou essa tela da cura do juiz do par. `app/docs/BACKLOG.md:775`, `app/docs/CORTES.md:318`, `app/docs/RELATO.md:1385`, `app/docs/PROMPTS.md:1558`.

- **C20-FIO-MUDO:** obra aberta na fila 1, evento na abertura de `batida_ausente` + cinco atos de perda. Seu universo é chamado `batida_ausente` **vivo**, sem pergunta viva; não equivale à classe `e-sem-viva`, que só ocorre após o ramo que classificaria chamado vivo como `a`. `app/docs/BACKLOG.md:1769`, `app/docs/RELATO.md:48`, `app/chamados/services/vigia_fio.py:664`, `app/chamados/services/vigia_fio.py:669`, `app/chamados/services/vigia_fio.py:685`.
- **C20 continua na raia:** registro mais recente informa suite verde, opus aprovou, pouso espera decisão de lei. O evento alcança zero dos 494/495 do estoque; estoque continua com cron, quatro dos cinco atos seguem sem evento e seis perguntas vivas sob outro chamado vivo permanecem incompletas. `app/docs/RELATO.md:6670`, `app/docs/RELATO.md:6671`, `app/docs/RELATO.md:6674`.
- **Decisão pendente C20:** reatar pergunta deve reabrir disputa fechada e produzir seus efeitos ou apenas apresentá-la ao admin com Validar, sem reabertura/push. `app/docs/RELATO.md:13`.
- **LEI-FIO-MUDO-B:** não encontrada no repositório nem nos quatro documentos solicitados; existência/estado/mecanismo **[nao verificado]**.

**Outras candidatas para e-sem-viva:**

| Candidata | Cobertura demonstrável | Limite |
|---|---|---|
| O261-FIO-MORTO-PELO-RETRATO | Mede classes a–e e exige cura por classe depois da medida; portanto é guarda-chuva documental da investigação. `app/docs/BACKLOG.md:963` | Estado descreve curas a/b/c e absorção pela O261-E; não documenta cura específica integral de `e-sem-viva`. `app/docs/BACKLOG.md:963`, `app/docs/BACKLOG.md:859` |
| O261 / GRUPO-D | Corte apensado trata lâmpada acesa sem pergunta/chamado: identificar emissor faltante e fazê-lo ler a lâmpada; núcleo inicial é `furo/FURO_SEM_COBRANCA`, no horizonte e não desligado. `app/docs/BACKLOG.md:1450` | A própria linha impede alargamento automático a `nunca_bateu`, `indefinida` e `fato_em_ausencia`; o fechamento em 24/10 refere-se ao contador então utilizado. `app/docs/BACKLOG.md:1450` |
| GRUPO-D-DIA-ACUSA-SEM-FIO | NO AR 25/10: `nunca_bateu` encaminhado ao DP; `sem_celula` remetido à SEM-CELULA-11. `app/docs/BACKLOG.md:1105` | Não demonstra que os 39 atuais pertençam às origens corrigidas. Alcance aos 39 **[nao verificado]** |
| SEM-CELULA-11-DEVE-TER-CELULA | Corrige gerador que excluía afastado INSS com vínculo ativo. `app/docs/BACKLOG.md:1620` | Ausência de célula é outro detalhe de `e`, `sem_celula`; não é `e-sem-viva`. `app/chamados/services/vigia_fio.py:667`, `app/chamados/services/vigia_fio.py:685` |

**Conclusão para consolidação:** há obras gerais e curas históricas por origem, mas **nenhuma cura específica registrada foi demonstrada cobrir integralmente os 39 `e-sem-viva`**; atribuição nominal desses casos a qualquer uma delas **[nao verificado]**. Os documentos mandam medir a origem antes da cura. `app/docs/BACKLOG.md:963`, `app/docs/BACKLOG.md:1450`, `app/docs/RELATO.md:2457`, `app/docs/RELATO.md:2458`.

## 06/11 08:54 · sessao f410b0a503a7 · - · - · origem=Ronald (tmux)

Definições e precedência:

- `d` é `colab_na_lista_cxr_dia_fora`: dia retido na folha, passado, sem porta, sem balde de lei, com célula, sem chamado vivo (senão cai em `a`), sem pergunta **viva**, com `dia_na_lista == (False, None)` e colaborador presente na lista da tela. O teste que atribui `d` está em `app/chamados/services/vigia_fio.py:677`; o comentário explicita que nenhum dono L-099 é lido em `app/chamados/services/vigia_fio.py:523`. As exclusões anteriores estão em `app/chamados/services/vigia_fio.py:642`, `app/chamados/services/vigia_fio.py:648`, `app/chamados/services/vigia_fio.py:654` e `app/chamados/services/vigia_fio.py:667`.
- `e-sem-viva` é o par `causa='e'`, `detalhe='sem_pergunta_viva'`: depois das exclusões anteriores e do teste de `d`, não há pergunta viva; também não caiu no caso “dia na lista com dono ESTRUTURA/nenhum”, que recebe detalhe próprio. A atribuição está em `app/chamados/services/vigia_fio.py:680`, `app/chamados/services/vigia_fio.py:681` e `app/chamados/services/vigia_fio.py:685`. Portanto, “sem pergunta viva” não significa necessariamente nunca ter tido pergunta: o teste com pergunta já validada também resulta nessa classe em `app/chamados/tests/test_o261e_f5_alcance.py:427`.
- As perguntas `ps` são exclusivamente as vivas fornecidas pelo juiz: `app/chamados/services/vigia_fio.py:610`; `alimentar_portas` usa `pergunta_viva` para montá-las em `app/chamados/juizes.py:1300`.
- A lista lida para distinguir `d` é a tela: `ListaDoAdmin._carregar` chama `escala.services.cadastro_realidade.lista`, deriva `dias_na_lista` e o conjunto de colaboradores em `app/chamados/services/vigia_fio.py:212`. `dia_na_lista` retorna `(True,dono)`, `(False,None)` ou `None` em falha, conforme `app/chamados/services/vigia_fio.py:230`; `colab_na_lista` lê presença em `app/chamados/services/vigia_fio.py:248`.
- Estar na lista com dono CADASTRO/BATIDA faz o dia sair antes das classes, para `fio_do_admin`: `app/chamados/services/vigia_fio.py:222`, `app/chamados/services/vigia_fio.py:313`. ESTRUTURA não satisfaz esse predicado: `app/chamados/services/vigia_fio.py:169`, `app/chamados/services/vigia_fio.py:228`.

Quem escreve e lê:

| Papel | Função e evidência |
|---|---|
| Escreve a classificação em memória | `censo_alcance`: cria contadores em `app/chamados/services/vigia_fio.py:571`; atribui `d` em `app/chamados/services/vigia_fio.py:677`; atribui `e-sem-viva` em `app/chamados/services/vigia_fio.py:685`; soma classe/detalhe em `app/chamados/services/vigia_fio.py:695`. |
| Escreve o detalhe por dia em memória | Função interna `_listar`, que acrescenta `[célula,colaborador,data,competência,rota,causa,detalhe,papel]`: `app/chamados/services/vigia_fio.py:625`; recebe classe/detalhe em `app/chamados/services/vigia_fio.py:699`. |
| Lê e formata classe/detalhe | `linhas_alcance`: `app/chamados/services/vigia_fio.py:703`, particularmente `app/chamados/services/vigia_fio.py:716`. |
| Lê o resultado e imprime | `Command.handle`: mede em `app/chamados/management/commands/vigiar_fio_morto.py:173`; imprime as linhas em `app/chamados/management/commands/vigiar_fio_morto.py:183`. |
| Persiste a fotografia contendo as classes | `Command.handle` entrega o bloco `alcance` integral ao carimbo em `app/chamados/management/commands/vigiar_fio_morto.py:211` e `app/chamados/management/commands/vigiar_fio_morto.py:223`; `_carimbar` grava JSON e substitui arquivo em `app/chamados/management/commands/vigiar_fio_morto.py:115`, `app/chamados/management/commands/vigiar_fio_morto.py:132`. |
| Lê o carimbo, sem consumir d/e especificamente | `bin/fio_estado.py::ler`: `bin/fio_estado.py:98`, `bin/fio_estado.py:104`; `_ler_carimbo` usa `alcance` em `bin/fio_estado.py:142`, mas expõe universo, cegueira e hoje/futuro em `bin/fio_estado.py:151`. |
| Lê para produzir tabela classe×dono do anexo | Implementação das sondas ausentes: **[nao verificado]**. |

Dono L-099 é outra pergunta: `InsumosDoDono.cxr` lê `dias_cadastro_x_realidade` do **motor** em `app/ponto/management/commands/e6_oraculo.py:154` e `app/ponto/management/commands/e6_oraculo.py:160`; `InsumosDoDono.dono` chama o juiz em `app/ponto/management/commands/e6_oraculo.py:172` e `app/ponto/management/commands/e6_oraculo.py:183`. A decisão CADASTRO/BATIDA/ESTRUTURA está em `app/ponto/management/commands/e6_oraculo.py:98`, com retorno final condicionado à presença no motor em `app/ponto/management/commands/e6_oraculo.py:110`.

O registro QUEIMA-A confirma a separação “CxR do MOTOR, não a da tela” para `d+ESTRUTURA` em `app/docs/RELATO.md:41`.

## 06/11 08:50 · sessao c44cc1260d0d · PEP-REENTRADA · verificador · origem=Code

**Não daria aceite à garantia de escritor único ainda:** o diff bloqueia o S143, mas existe outra cadeia síncrona que pode fechar a mesma disputa e acionar o S144, cujo carimbo não consulta `_EM_VOO`. (`app/chamados/reconciliador.py:602`; `app/chamados/signals.py:140`; `app/chamados/services/materializacao.py:1395`)

A análise abaixo é estática. Execução dos testes, mutantes, reprodução em produção e confirmação independente do hash contra o pai: **[não verificado]**. [nao verificado]

1) **Quem escreve e quem lê os estados.**

| Estado | Escritores | Leitores e finalidade |
|---|---|---|
| `PerguntaDisputa.materializada_em` | A atribuição individual central é `_escrever_carimbo`; `carimbar`, `carimbar_identidade` e `descarimbar` passam por ela. O descarimbo em lote escreve por `update`. (`app/chamados/models.py:2556`; `app/chamados/models.py:2586`; `app/chamados/models.py:2636`; `app/chamados/models.py:2648`; `app/chamados/models.py:2663`) | C6, juiz do lastro, estado da pergunta, seleção do materializador/S144 e seleção do S143. (`app/chamados/juizes.py:647`; `app/chamados/juizes.py:1177`; `app/chamados/juizes.py:1207`; `app/chamados/models.py:2499`; `app/chamados/services/materializacao.py:1343`; `app/chamados/services/materializacao.py:1390`; `app/chamados/services/disputa_emissao.py:1574`) |
| `materializada_por` | As mesmas portas, junto do instante e da via; autor sem `pk` vira autor nulo. (`app/chamados/models.py:2559`; `app/chamados/models.py:2582`; `app/chamados/models.py:2639`; `app/chamados/models.py:2664`) | Snapshot/restauração da porta, procedência da batida, agrupamento/foto da absorção por ausência e auditoria. Não integra os predicados C6/lastro mostrados. (`app/chamados/models.py:2583`; `app/ponto/services/procedencia_batida.py:71`; `app/chamados/services/materializacao.py:654`; `app/chamados/reconciliador.py:1134`; `app/chamados/juizes.py:1135`; `app/chamados/juizes.py:632`) |
| `via_resolucao` | As portas do trio; também há escritores preexistentes só da via: `encerrar_por_fato_no_espelho` e `relavra_via.relavrar`. O diff não cria atribuição nova desse campo. (`app/chamados/models.py:2560`; `app/chamados/models.py:2673`; `app/chamados/services/relavra_via.py:123`; `app/chamados/services/materializacao.py:1248`) | C6/lastro, explicação de estado, recusa, relavra e trilha do S143. (`app/chamados/juizes.py:647`; `app/chamados/juizes.py:1145`; `app/chamados/juizes.py:1171`; `app/chamados/services/explicador.py:95`; `app/chamados/services/materializacao.py:1064`; `app/chamados/services/relavra_via.py:116`; `app/chamados/services/disputa_emissao.py:1676`) |
| `DisputaSupervisao.fechada_em` / `resolucao` | O fechamento grava ambos em `fechar`. Reabertura administrativa e sistêmica limpam ambos; `limpar_resolucao_residual` limpa apenas o texto de disputa aberta. (`app/chamados/models.py:1861`; `app/chamados/models.py:2063`; `app/chamados/models.py:2087`; `app/chamados/models.py:2173`) | Estado aberto, idempotência, seleção/cauda do S143, cauda do materializador, receivers e apresentação do fio. A resolução também é fallback do texto do fechamento granular. (`app/chamados/models.py:1765`; `app/chamados/models.py:1820`; `app/chamados/services/disputa_emissao.py:1575`; `app/chamados/services/disputa_emissao.py:1710`; `app/chamados/services/materializacao.py:1287`; `app/chamados/services/materializacao.py:1299`; `app/chamados/signals.py:28`; `app/chamados/signals.py:45`; `app/chamados/services/fio.py:131`) |
| `_EM_VOO` | Criado com conjunto vazio; `materializacao_em_voo` instala a união dos `pk` e restaura o token no `finally`. (`app/chamados/services/materializacao.py:1103`; `app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1121`) | `pergunta_em_voo`; em produção, seus consumidores são as três guardas acrescentadas ao S143. (`app/chamados/services/materializacao.py:1126`; `app/chamados/services/disputa_emissao.py:1586`; `app/chamados/services/disputa_emissao.py:1671`; `app/chamados/services/disputa_emissao.py:1710`) |

Os **oito sítios de chamada de `carimbar` em produção** são estes; as chamadas dos testes não constituem escritores adicionais de produção. (`app/chamados/models.py:2685`; `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:586`; `app/chamados/services/disputa_emissao.py:1612`; `app/chamados/services/disputa_emissao.py:1686`; `app/chamados/services/materializacao.py:549`; `app/chamados/services/materializacao.py:1248`; `app/chamados/services/materializacao.py:1395`)

| Sítio | Alcançabilidade durante o bloco e efeito sobre a P em voo |
|---|---|
| Materializador, `app/chamados/services/materializacao.py:1248` | **Direta:** é o carimbo final pretendido, com declaração ou via do fato, depois do juiz do lastro. (`app/chamados/services/materializacao.py:1203`; `app/chamados/services/materializacao.py:1232`) |
| S143 principal, `app/chamados/services/disputa_emissao.py:1612` | **Síncrona:** reconciliação do destino → `post_save` → S143. Agora pula a P em voo antes de chegar ao carimbo. (`app/chamados/services/materializacao.py:746`; `app/chamados/services/materializacao.py:949`; `app/chamados/signals.py:75`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1586`) |
| S143 sem credor, `app/chamados/services/disputa_emissao.py:1686` | **Mesmo receiver:** agora também pula a P em voo; normalmente a P validada já estaria excluída de `_abertas`. (`app/chamados/services/disputa_emissao.py:1665`; `app/chamados/services/disputa_emissao.py:1671`) |
| S144, `app/chamados/services/materializacao.py:1395` | **Síncrona se alguma rota fechar a disputa:** não consulta voo; carimba `VIA_JUIZ_APROVOU` quando o lastro aprova. (`app/chamados/signals.py:124`; `app/chamados/signals.py:140`; `app/chamados/services/materializacao.py:1391`) |
| Absorção por ausência, `app/chamados/services/materializacao.py:549` | Não é chamada diretamente por `_materializar_uma`; é chamada pelo escritor da resposta. Se executada reentradamente para o mesmo dia, não tem guarda de voo e tenta carimbar as perguntas-hora selecionadas. Aresta canônica do plantio até esse escritor da resposta: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1330`; `app/chamados/services/materializacao.py:531`) |
| Sem turno aberto, `app/chamados/reconciliador.py:405` | **Alcançável pelo signal da batida**, via reconciliação de órfãs. A P-hora normal com texto é barrada pela guarda de resposta; outras perguntas continuam elegíveis. (`app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1417`; `app/chamados/reconciliador.py:1603`; `app/chamados/reconciliador.py:364`) |
| Célula absolveu, `app/chamados/reconciliador.py:586` | **Alcançável pelo signal da batida.** Exclui diretamente a P validada, mas pode carimbar uma irmã e fechar a mesma disputa, chegando indiretamente ao S144. (`app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1416`; `app/chamados/reconciliador.py:620`; `app/chamados/reconciliador.py:602`) |
| `encerrar_fantasma`, `app/chamados/models.py:2685` | Chamado pelo consumidor de eventos da célula; não encontrei ligação síncrona desse consumidor ao bloco revisado. Não tem guarda de voo própria. (`app/chamados/reconciliador.py:1105`; `app/chamados/reconciliador.py:1136`; `app/chamados/reconciliador.py:1363`) |

Há ainda os carimbos **de identidade**, fora desse inventário de chamadas a `carimbar`: paridade e colisão pertencem ao materializador e usam sua porta própria; não representam proteção contra outro escritor reentrante. (`app/chamados/services/materializacao.py:899`; `app/chamados/services/materializacao.py:1074`; `app/chamados/models.py:2625`)

Os **quinze sítios de chamada de fechamento de `DisputaSupervisao` em produção** encontrados são:

| Sítio | Dentro do bloco revisado? Consequência para a P em voo |
|---|---|
| `app/chamados/services/disputa_emissao.py:1713` | **Sim**, S143 síncrono; a guarda nova impede fechar disputa com P em voo. (`app/chamados/services/disputa_emissao.py:1710`) |
| `app/chamados/reconciliador.py:602` | **Sim**, signal da batida; sem guarda de voo, pode disparar S144 sobre P. (`app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1416`; `app/chamados/signals.py:140`) |
| `app/chamados/reconciliador.py:412` | **Sim**, reconciliação de órfãs; a P normal com texto não é lavrada ali, mas o fecho ocasionado por outra pergunta não tem guarda de voo. (`app/chamados/reconciliador.py:364`; `app/chamados/reconciliador.py:1603`) |
| `app/chamados/services/disputa_emissao.py:1118` | **Condicionalmente**, pelo emissor acionado pelo cartório durante o signal da batida; esse fecho usa C7 e não consulta voo. (`app/ponto/signals.py:135`; `app/ponto/services/cartorio.py:1989`; `app/ponto/services/cartorio.py:1740`; `app/ponto/management/commands/detectar_ausencias.py:801`; `app/chamados/services/disputa_emissao.py:1112`) |
| `app/chamados/services/materializacao.py:1304` | No fluxo normal, **depois** das unidades e respectivos marcadores; numa chamada pública aninhada, pode executar enquanto uma unidade externa ainda está em voo. Não consulta voo. (`app/chamados/services/materializacao.py:1346`; `app/chamados/services/materializacao.py:1349`; `app/chamados/services/materializacao.py:1287`) |
| `app/chamados/services/materializacao.py:568` | Rota da resposta/ausência, sem guarda de voo; ligação síncrona canônica ao plantio: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1330`) |
| `app/chamados/services/becos.py:52` | O receiver agenda sua entrada por `on_commit`: normalmente roda depois do bloco, sem a marca. (`app/chamados/signals.py:197`; `app/chamados/signals.py:202`) |
| `app/chamados/reconciliador.py:1160` | Consumidor do evento da célula; ligação síncrona ao bloco: **[não verificado]**. (`app/chamados/reconciliador.py:1105`; `app/chamados/reconciliador.py:1363`) |
| `app/chamados/models.py:1879` | Porta `admin_encerra`; ligação a partir do plantio: **[não verificado]**. Se chamada, pode carimbar perguntas vivas por decisão administrativa antes do fechamento. (`app/chamados/models.py:1852`; `app/chamados/models.py:1855`) |
| `app/chamados/models.py:1986` | Remoção do último motivo; os chamadores incluem signals de colaborador/escala. Ligação desses eventos ao plantio revisado: **[não verificado]**. Essa porta apaga as perguntas do motivo antes de fechar. (`app/colaboradores/signals.py:77`; `app/escala/signals.py:161`; `app/chamados/models.py:1976`) |
| `app/api/foto_recebida.py:80` | Rota de upload; ligação síncrona ao plantio: **[não verificado]**. |
| `app/ponto/services/entrada_sincronizada.py:59` | Rota chamada pelas APIs de sync; ligação a partir do plantio: **[não verificado]**. (`app/api/views.py:805`; `app/api/views_core.py:805`) |
| `app/chamados/services/competencia_trancada.py:204` | Porta da tranca; ligação síncrona ao plantio: **[não verificado]**. |
| `app/colaboradores/services/desligamento.py:142` | Porta de desligamento; ligação síncrona ao plantio: **[não verificado]**. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | Comando; ligação síncrona ao plantio: **[não verificado]**. |

O contraexemplo principal não depende das rotas marcadas como não verificadas:

- O plantio grava a batida e aciona o reconciliador sincronamente. (`app/chamados/services/materializacao.py:902`; `app/ponto/signals.py:206`; `app/ponto/signals.py:215`)
- Uma irmã Q, não validada, sem carimbo e com célula julgada que não acusa, pode entrar na absolvição e ser carimbada. (`app/chamados/reconciliador.py:617`; `app/chamados/reconciliador.py:627`; `app/chamados/reconciliador.py:586`)
- A P em voo, por estar validada, já é morta pelo C6; portanto não impede C7 de liberar o fechamento da mesma disputa. (`app/chamados/juizes.py:1136`; `app/chamados/models.py:1794`; `app/chamados/reconciliador.py:602`)
- O fechamento aciona S144; o lastro pode encontrar a batida recém-gravada mesmo antes de ela receber `pergunta_origem`. S144 carimba P com `VIA_JUIZ_APROVOU`; depois o materializador pode carimbá-la novamente com declaração. (`app/chamados/signals.py:140`; `app/chamados/juizes.py:774`; `app/chamados/services/materializacao.py:921`; `app/chamados/services/materializacao.py:1395`; `app/chamados/services/materializacao.py:1248`)

É uma possibilidade demonstrada pela composição do código; fixture executada e ocorrência produtiva: **[não verificado]**.

2) **Escopo do sinal.**

| Situação | Resultado |
|---|---|
| Saída normal/commit | O `reset` executa **antes** da saída do `atomic`, pela ordem inversa dos context managers. Se houver transação externa, o sinal termina mesmo sem commit físico naquele ponto. (`app/chamados/services/materializacao.py:1191`; `app/chamados/services/materializacao.py:1121`) |
| `_Reprovada` | A exceção atravessa os context managers: restaura o token, desfaz o atomic e só então é tratada. (`app/chamados/services/materializacao.py:1210`; `app/chamados/services/materializacao.py:1120`; `app/chamados/services/materializacao.py:1254`) |
| Outra exceção | Mesmo `finally`; `Exception` é acumulada como erro fora do bloco. Exceções que não forem capturadas também atravessam o `finally`. (`app/chamados/services/materializacao.py:1120`; `app/chamados/services/materializacao.py:1256`) |
| `return` interno | Também passa pelo `finally`. (`app/chamados/services/materializacao.py:1199`; `app/chamados/services/materializacao.py:1120`) |
| Rollback de savepoint interno | Não remove a marca externa: ela pertence ao context manager externo, não ao savepoint. Se houver marcador interno, sua saída restaura o conjunto anterior. (`app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1121`) |
| Outra P aninhada | União dos dois `pk`; ao sair da interna, restaura somente a externa. (`app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1121`) |
| Mesma P aninhada | O conjunto continua contendo o mesmo `pk`; sair da interna preserva a marca externa. **Não impede materializar/carimbar a mesma P novamente.** (`app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1129`; `app/chamados/services/materializacao.py:1248`) |

No fluxo síncrono mostrado, não há saída do bloco que dispense a restauração do token. O objeto é de módulo, mas o valor é de contexto; não é um conjunto global compartilhado entre as quatro threads configuradas do gthread. (`app/chamados/services/materializacao.py:1103`; `app/chamados/services/materializacao.py:1120`; `app/gunicorn.conf.py:19`)

O `on_commit` normal, executado depois desse bloco, não vê a marca criada por ele, porque ela já foi restaurada. Outra request/thread independente ou task/cron separado não recebe automaticamente esse valor. (`app/chamados/services/materializacao.py:1191`; `app/chamados/services/materializacao.py:1121`; `app/chamados/signals.py:121`; `app/chamados/signals.py:202`)

A promessa absoluta “nenhum caminho sobrevive” exige ressalva: um contexto explicitamente copiado ou task que herde o contexto pode conservar um snapshot que o `reset` do contexto original não limpa. O sinal contém apenas `pk`, sem identidade da transação. Caminho concreto de cópia/herança desse contexto na aplicação: **[não verificado]**. (`app/chamados/services/materializacao.py:1103`; `app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1121`)

3) **Aliases e conexões.**

**Não é genericamente correto para múltiplos aliases.** `_em_atomic` consulta somente `django.db.connection`, isto é, a conexão padrão; o materializador e o receiver S143 também abrem `atomic()` sem `using`, e o receiver não encaminha o alias recebido pelo signal. (`app/chamados/services/materializacao.py:1106`; `app/chamados/services/materializacao.py:1191`; `app/chamados/signals.py:76`; `app/chamados/signals.py:91`)

Assim, um atomic só no alias B não satisfaz a guarda; inversamente, estar em atomic no padrão não prova que uma operação feita em B pertença à transação protegida. O conjunto também não distingue dois objetos com mesmo `pk` em bancos diferentes. (`app/chamados/services/materializacao.py:1108`; `app/chamados/services/materializacao.py:1117`; `app/chamados/services/materializacao.py:1126`)

Na configuração SaaS da cópia há apenas `default`, com tenants por schema. Os chamadores encontrados do S143 não selecionam alias na chamada; execução efetiva em conexão diferente ou configuração produtiva divergente: **[não verificado]**. (`app/config/settings/saas.py:83`; `app/config/settings/saas.py:93`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:1631`; `app/chamados/services/becos.py:49`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`)

4) **O `continue` da cauda e o aval.**

Minha leitura é que ele é **compatível como bloqueio de uma rota indireta de carimbo**: impedir só o laço principal deixaria a cauda fechar D e S144 carimbar P. Não cria escritor novo, mas muda observavelmente momento, autor e texto do fechamento; é uma decisão adicional de ordenação, não um resultado de C7. (`app/chamados/services/disputa_emissao.py:1710`; `app/chamados/services/disputa_emissao.py:1713`; `app/chamados/signals.py:140`; `app/chamados/services/materializacao.py:1395`)

O caminho normal posterior é `_fechar_disputa_se_quitada`, depois do laço. O texto será `resolucao_pedida`, senão a resolução já existente na instância, senão `Regularizacao granular: N pergunta(s) materializada(s).` Portanto, “será sempre a resolução granular” seria incorreto. (`app/chamados/services/materializacao.py:1349`; `app/chamados/services/materializacao.py:1299`; `app/chamados/services/materializacao.py:1304`)

O `continue` não deixa veto persistente: a marca desaparece, e a cauda normal ainda consulta C7. Se outra pergunta permanecer viva ou com resposta sem veredito, a disputa permanece aberta por esse juiz; sua resolução futura depende de outro ato/evento, cuja ocorrência é **[não verificado]**. (`app/chamados/services/materializacao.py:1121`; `app/chamados/services/materializacao.py:1287`; `app/chamados/models.py:1794`)

Se o materializador reprovar, o fecho S143 que antes ocorresse **dentro do mesmo atomic** também seria revertido; pulá-lo não elimina um fechamento que sobreviveria legitimamente ao rollback. A porta de validação possui tratamento posterior para devolver validação recusada e reabrir a disputa. (`app/chamados/services/materializacao.py:1191`; `app/chamados/services/materializacao.py:1210`; `app/chamados/services/validacao.py:425`; `app/chamados/services/validacao.py:199`)

Há uma distinção importante: P validada pode ser considerada morta pelo C6 mesmo sem carimbo. Logo a cauda pode fechar após uma tentativa malsucedida; “P sem `materializada_em` implica disputa necessariamente aberta” não é lei desse código. (`app/chamados/juizes.py:1136`; `app/chamados/services/materializacao.py:1287`)

Não identifiquei abertura eterna causada exclusivamente por esse `continue`; tampouco o código garante progresso futuro sem novos atos. A afirmação do comentário de que “quem fecha essa disputa é o materializador” é ampla demais diante do fecho do reconciliador descrito no item 1. (`app/chamados/services/disputa_emissao.py:1708`; `app/chamados/reconciliador.py:602`)

5) **Contornos.**

| Caso | Resultado por leitura |
|---|---|
| `hora` | Recebe o marcador antes do ramo; reconcilia destino/clones antes do carimbo final. (`app/chamados/services/materializacao.py:1191`; `app/chamados/services/materializacao.py:1193`; `app/chamados/services/materializacao.py:949`; `app/chamados/services/materializacao.py:1248`) |
| `data_hora` | Mesma proteção; destino é opcional e a reconciliação ocorre no fim do ramo. (`app/chamados/services/materializacao.py:1195`; `app/chamados/services/materializacao.py:969`; `app/chamados/services/materializacao.py:1034`) |
| `escolha` | Mesma proteção; pode declarar ausência e reconciliar destinos/clones, sem exigir o juiz de batida usado 

[truncado em 20000 de 31138 caracteres]

## 06/11 08:47 · sessao b0d20fd3ef34 · - · - · origem=Ronald (tmux)

Inventário de produção: **8 chamadas de `PerguntaDisputa.carimbar` e 15 chamadas de `DisputaSupervisao.fechar`** encontradas na cópia, discriminadas abaixo; testes e migrações ficam fora deste inventário. Os próprios sítios são as referências de evidência. [nao verificado]

### Todos os `carimbar`

| Sítio | Alcançabilidade no bloco de `_materializar_uma` / efeito sobre P |
|---|---|
| `app/chamados/services/materializacao.py:1248` | **Direta**: escritor pretendido, com `VIA_DECLARACAO` quando `declarou_hora(p)`, conforme `:1232-1249`. |
| `app/chamados/services/disputa_emissao.py:1612` | **Síncrona**, por fecho de destino/clones → `post_save` → S143 (`app/chamados/services/materializacao.py:949`; `app/chamados/signals.py:75-94`). Agora pula P em voo em `disputa_emissao.py:1587`. |
| `app/chamados/services/disputa_emissao.py:1686` | Mesmo S143, ramo `_abertas`; nova guarda em `:1671`. Além disso `_abertas` exclui validada (`:1666`), portanto a P canônica validada já não entrava. |
| `app/chamados/services/materializacao.py:1395` | **Síncrona indireta** se qualquer caminho fechar D durante plantio: `app/chamados/signals.py:124-140`. **Não consulta em voo**; pode carimbar P em `VIA_JUIZ_APROVOU` antes de `:1248`. |
| `app/chamados/reconciliador.py:586` | **Síncrona** por signal da Batida (`app/ponto/signals.py:206-215`, `reconciliador.py:1416`). Não atinge a P canônica validada: filtro `validada_em__isnull=True` em `reconciliador.py:620`. Pode carimbar irmã Q e fechar a mesma D em `:602`, disparando S144 sobre P. |
| `app/chamados/reconciliador.py:405` | **Síncrona** via reconciliação de órfãs (`:1417`, `:1493-1511`, `:1603`); P com resposta textual é excluída por `:364-365`, portanto a P canônica `hora` não recebe esse carimbo. Sem guarda `_EM_VOO`. |
| `app/chamados/models.py:2685` | Via `encerrar_fantasma`, chamada pelo consumidor de EventoCelula (`reconciliador.py:1136`, `:1363`). Não encontrei aresta desse consumidor no fluxo síncrono do plantio; consumidor descrito como cron em `reconciliador.py:1105`. Sem guarda `_EM_VOO`. |
| `app/chamados/services/materializacao.py:549` | Ausência declarada por resposta: chamador `disputa_emissao.py:1327-1330`, **antes e fora do caminho `_materializar_uma`**. `_materializar_resposta_escolha` chama outro helper de ausência (`materializacao.py:1052-1054`). Sem guarda `_EM_VOO`; entrada explícita/aninhada poderia atingir P `hora` ainda nula pelo filtro `:531-532`. |

### Achado principal: bypass concreto do S144

Condições suficientes por leitura:

1. P validada, ainda não materializada; Q irmã, mesma disputa, não validada/não materializada, com célula julgada que não acusa (`app/chamados/reconciliador.py:617-631`).
2. Plantio de P cria Batida (`app/chamados/services/materializacao.py:902`; `app/ponto/registro_batida.py:157-159`). `post_save` julga a célula e depois reconcilia chamados (`app/ponto/signals.py:131-135`, `:206-215`).
3. Q passa no C6 e recebe carimbo `celula_absolveu` (`reconciliador.py:644-662`, `:586`); a disputa pode fechar em `:602`.
4. P **não segura C7** porque `validada_em` mata a vitalidade imediatamente (`app/chamados/juizes.py:1135-1137`); C7 só combina ausência de resposta sem veredito e fio vivo (`app/chamados/models.py:1783-1794`).
5. Fechar D dispara S144 imediatamente (`app/chamados/signals.py:124-140`), que considera P ainda nula e não verifica `_EM_VOO` (`materializacao.py:1389-1395`).
6. Lastro pode aprovar P **antes do vínculo `pergunta_origem`**: o juiz tenta origem, mas aceita a batida viva na janela (`app/chamados/juizes.py:718-728`, `:774-776`). A origem só é anotada depois do primeiro `registrar_batida` (`materializacao.py:921-925`).
7. P recebe `VIA_JUIZ_APROVOU` e depois o materializador torna a chamar `carimbar` em `materializacao.py:1248`; a porta não exige que o carimbo anterior seja nulo (`models.py:2583-2609`).

É caminho estático reproduzível sob as condições acima; ocorrência real e execução desse cenário **[não verificado]**.

### Todos os fechos de disputa

| Sítio | Dentro do bloco? / consequência |
|---|---|
| `app/chamados/services/disputa_emissao.py:1713` | Sim, S143; agora pula D com qualquer P em voo em `:1710-1711`. |
| `app/chamados/reconciliador.py:602` | **Sim**, pela Batida e por eventual E2 dentro do cartório (`app/ponto/services/cartorio.py:958-959`); sem guarda em voo, pode atingir mesma D e acionar S144. |
| `app/chamados/reconciliador.py:412` | **Sim**, pela reconciliação de órfãs (`:1417`, `:1603`); pode fechar D após carimbar outra P, sem guarda em voo. |
| `app/chamados/services/disputa_emissao.py:1118` | **Condicionalmente sim**: cartório chama emissor em `app/ponto/services/cartorio.py:1740`/`:1778`; emissor abre fio por `app/ponto/management/commands/detectar_ausencias.py:787-801` → `criar_disputa_a_partir_de_chamados_orfaos`. Não é a aresta da `fabrica_da_celula`; esta chama outra função em `fabrica_da_celula.py:139`. Sem guarda em voo. |
| `app/chamados/services/materializacao.py:1304` | No fluxo normal **depois** do bloco, ao terminar o laço (`:1346-1350`). Sob reentrada aninhada de outro materializador, poderia ocorrer enquanto a marca externa continua ativa. |
| `app/chamados/services/materializacao.py:568` | Resposta de ausência em `disputa_emissao.py:1330`, não chamada por `_materializar_uma`; não encontrei aresta síncrona a partir do bloco. Sem guarda em voo. |
| `app/chamados/services/becos.py:52` | Callback agendado pelo save de Pergunta (`app/chamados/signals.py:191-202`); execução normal após sair da transação/bloco. Recorte/juiz em `becos.py:25-33`, `:48-52`. |
| `app/chamados/reconciliador.py:1160` | Consumidor de EventoCelula (`:1363`), não encontrei aresta dentro do bloco; sem guarda em voo. |
| `app/chamados/models.py:1879` | Porta humana `admin_encerra`; não encontrei aresta a partir do plantio. Fecha pela exceção e pode `fechar_por_admin` vivas (`:1852-1855`). |
| `app/chamados/models.py:1986` | Remoção do último motivo. Callers nos signals de Posto (`app/colaboradores/signals.py:77`) e da âncora de Escala (`app/escala/signals.py:161`); plantio de `data_hora` explicitamente não grava mais fase (`materializacao.py:1001-1004`). Não encontrei aresta do plantio a esses eventos. |
| `app/api/foto_recebida.py:80` | Upload de foto; não encontrei aresta do plantio. |
| `app/ponto/services/entrada_sincronizada.py:59` | Sync API: `app/api/views.py:805`, `views_core.py:805`; não encontrei aresta do plantio. |
| `app/chamados/services/competencia_trancada.py:204` | Aplicação de tranca; não encontrei aresta do plantio. |
| `app/colaboradores/services/desligamento.py:142` | Desligamento; não encontrei aresta do plantio. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | Comando do acervo; não encontrei aresta do plantio. |

Todos os fechos que chegam a `DisputaSupervisao.fechar` fazem o save de D (`models.py:1861-1865`) e portanto podem acionar o S144 síncrono (`signals.py:124-140`); a proteção atual está exclusivamente no chamador S143 (`disputa_emissao.py:1710`).

### Campos: escritores e leitores

- **Trio de carimbo**: atribuição central em `app/chamados/models.py:2556-2560`; chamado por `carimbar` (`:2583-2609`), identidade (`:2636-2644`), descarimbar (`:2646-2652`). Lote limpa os três juntos (`:2663-2664`). Escritores isolados existentes de via: `encerrar_por_fato_no_espelho` (`:2673-2674`) e relavra (`app/chamados/services/relavra_via.py:116-126`).
- **Fechada_em/resolucao**: fecho escreve juntos em `models.py:1861-1865`; reabrir admin limpa em `:2063-2075`; sistema em `:2087-2091`; `limpar_resolucao_residual` limpa só texto de disputa aberta (`:2170-2174`). Assim “só fechar escreve resolucao” é inexato sem mencionar as portas de reabertura/limpeza.
- **Leitores semânticos de materializada_em/via**: C6 (`app/chamados/juizes.py:1145`, `:1171`, `:1177`, `:1207-1214`), lastro (`:647-673`, `:718-778`), assinatura de memo (`:813-814`), estado de Pergunta (`models.py:2499-2505`), fila do materializador (`materializacao.py:1340-1343`), S144 (`:1389-1395`), agrupamento de ausência (`:607-614`), aplicação UI (`:433-441`), S143 (`disputa_emissao.py:1574`, `:1666`).
- **Materializada_por**: autoria/snapshot/trilha, não condição de C6/lastro: `models.py:2583`, `:2636-2639`; `materializacao.py:654`; `reconciliador.py:1134`, `:1144`; procedência da Batida escolhe `validada_por or materializada_por` (`app/ponto/services/procedencia_batida.py:71-74`).
- **Outros leitores de apresentação/auditoria**: `app/chamados/services/explicador.py:95` via; `historico.py:103` instante; `painel.py:67` instante agregado; `raiox.py:133`, `:173`; `passivo_validado.py:64`, `:140-142`; `app/ponto/services/flip_auto.py:129`.
- **Fechada_em**: propriedade aberta (`models.py:1765`), idempotência de fecho (`:1820-1824`), reabrir (`:2052`, `:2081`), seleção e cauda S143 (`disputa_emissao.py:1630`, `:1654`, `:1656`, `:1710`), cauda materializador (`materializacao.py:1287`), gates/seleção de fio (`fio.py:131`, `:135`, `:184`, `:596`, `:629`), validação (`validacao.py:196-201`), UI (`app/api/views.py:1008`, `:1030`, `:2694`; `app/chamados/views.py:471`, `:495`, `:512`, `:530`, `:911`, `:1329`).
- **Resolucao**: idempotência (`models.py:1821`), fallback materializador (`materializacao.py:1299-1300`), texto do fio após fecho (`app/chamados/signals.py:45`), limpeza residual (`models.py:2170-2173`).
- **`_EM_VOO`**: definição `materializacao.py:1102`; `.set` no contexto `:1117`; `.reset` em `:1121`; leitura no predicado `:1126`; consumidores S143 `disputa_emissao.py:1587`, `:1671`, `:1710`. Não é fonte consultada por C6, lastro, reconciliador ou S144 nesses respectivos sítios.

## 06/11 08:46 · sessao 802b8b8173a0 · - · - · origem=Ronald (tmux)

Auditoria estática; execução de testes e mutantes: [não verificado]. [nao verificado]

**Mordida e mutantes sobreviventes, teste a teste** [nao verificado]

| Teste (`app/chamados/tests/test_pep_reentrada.py`) | O que realmente observa; mutante que pode continuar verde |
|---|---|
| `test_MORDE_a_P_em_voo_tem_UM_escritor_o_materializador` — :83 | Observa destino terminado, existência de Batida e exatamente um `carimbar` **bem-sucedido** com `VIA_DECLARACAO` (:87-92). Morde a remoção da guarda principal e, indiretamente, da cauda: sem a cauda, fechar dispara S144, que carimba `VIA_JUIZ_APROVOU` antes do materializador (`signals.py:139-140`; `materializacao.py:1395`, :1248). **Sobrevive:** desativar o receiver S143 inteiro; destino e materializador ainda podem cumprir as três asserções, e os testes diretos de S143 não verificam a ligação do receiver (:74-76, :169-182; `signals.py:75-92`). O spy ignora tentativas que retornam `False`, `carimbar_identidade` e escrita direta (:69-73). | [nao verificado]
| `test_a_via_gravada_e_a_do_materializador` — :94 | Só vê via/autor finais (:98-99). **Sobrevive:** remover todas as guardas de voo do S143; o carimbo final do materializador regrava declaração/user (`materializacao.py:1248-1249`), pois `carimbar` não protege carimbo já gravado e atribui/grava novamente (`models.py:2583-2586`, :2609). É estruturalmente incapaz de detectar o segundo escritor antigo. |
| `test_o_S143_nao_fecha_a_disputa_em_nome_dele_no_meio_do_plantio` — :101 | Só vê disputa e resolução **finais**, sem observar o meio do plantio (:104-107). **Sobrevive:** remover a guarda da cauda; o S143 fecha uma instância obtida pela consulta/refresh (`disputa_emissao.py:1616`, :1703, :1713), mas o `d` original do materializador segue com `fechada_em` em memória. A cauda do materializador não atualiza esse `d` e pode fechar novamente com texto granular (`materializacao.py:1287`, :1299-1304); `fechar` também verifica o estado da própria instância, não busca estado atual (`models.py:1820-1824`, :1861-1865). Logo a resolução S128 intermediária pode ser sobrescrita, deixando este teste verde também no fluxo antigo. | [nao verificado]
| `test_o_sinal_existe_DURANTE_o_plantio_e_morre_na_saida` — :112 | Observa sinal na entrada de `_materializar_resposta_hora` e após sucesso (:118-124). **Sobrevive:** reset apenas no sucesso, com vazamento em exceção; não há exceção neste cenário. Não prova permanência até o carimbo nem fronteira de commit real (:118-124; :1248). | [nao verificado]
| `test_MORDE_o_sinal_morre_no_ROLLBACK` — :126 | Força juiz de lastro falso, vê P sem carimbo e getter falso (:129-133). **Sobrevive:** o materializador deixar de instalar o sinal completamente; a propriedade “não sobrevive” passa sem jamais tê-lo visto nascer. Não verifica Batida/destino efetivamente revertidos (:131-133). A origem `_Reprovada` está em `materializacao.py:1203-1210`. |
| `test_MORDE_o_sinal_morre_na_EXCECAO_que_atravessa` — :135 | Testa o context manager diretamente e vê getter falso após `_Boom` (:141-144). **Sobrevive:** context manager só fazer `yield`, sem instalar sinal; falta asserção positiva antes de levantar. Não testa exceção real atravessando `_materializar_uma`, que captura `Exception` (`materializacao.py:1256-1257`). |
| `test_aninhado_cada_P_e_a_sua` — :146 | Cobre duas P distintas, duas entradas/saídas normais e restauração do pai (:150-155). **Sobrevive:** implementar conjunto global mutável com add/remove por P; neste cenário síncrono distinto ele produz os mesmos resultados. Não cobre mesma P aninhada, reentrada real de `_materializar_uma`, exceção aninhada, isolamento entre contextos ou rollback de savepoint (:150-155). |
| `test_P_fora_de_voo_o_S143_segue_carimbando` — :167 | Controle positivo deliberado: chamada direta, uma P e batida real (:157-174). Passar no código antigo é esperado; não é RED da cura. **Sobrevive:** retirar `_em_atomic()` do getter; nenhuma P foi instalada neste cenário (:170-174; `materializacao.py:1126`). Não exige via nem autor (:173-174). |
| `test_MORDE_P_em_voo_o_S143_pula` — :176 | Chamada direta com sinal; exige contador zero e P sem carimbo (:181-185). Morde retirada da guarda principal. **Sobrevive:** S143 pular **todas** as perguntas sempre que houver qualquer voo; o cenário tem uma P, sem controle simultâneo de outra P (:157-165, :180-185). |
| `test_MORDE_fora_de_atomic_recusa` — :190 | Exige exceção sem atomic e getter falso (:194-197). **Sobrevive:** o context manager sempre levantar `RuntimeError`, inclusive dentro de atomic; este teste só usa a borda proibida. Também não distingue `_EM_VOO` limpo de valor escondido pelo `and _em_atomic()` (`materializacao.py:1126`). | [nao verificado]

**Vacuidade e alcance**

- Os testes de via final (:94-99) e resolução final (:101-107) não comprovam o defeito intermediário: o próprio fluxo pode sobrescrever a evidência; as respectivas sobrescritas estão em `materializacao.py:1248-1249`, :1299-1304 e `models.py:1861-1865`, :2586-2609. A confirmação de verde no pai: [não verificado]. [nao verificado]
- O RED forte é a lista de carimbos bem-sucedidos (:69-73, :90-92), que preserva evidência do escritor intermediário mesmo após sobrescrita final. Ele não comprova sozinho que S143 foi realmente chamado, pois o receiver pode ser desligado (`signals.py:75-92`).
- A fixture só prepara `hora`, uma P validada, um destino inicialmente aberto (:43-62). Não há casos de `data_hora`, `escolha`, sem destino, destino já fechado, clones com outras P, ausência/S144 invocado por outro caminho, alias diferente, mesma P aninhada, thread/contexto copiado ou savepoint interno revertido (:43-62, :83-197).
- O helper executa `captureOnCommitCallbacks(execute=True)` num `TestCase` (:36, :74-76); não constitui observação de commit real em `TransactionTestCase`. Comportamento de commit real nessa suíte: [não verificado]. [nao verificado]

**Classes de reprovação**

- **Segundo juiz:** C6 e C7 vivos são `pergunta_viva` e `DisputaSupervisao.pode_fechar` (`app/core/juizes.py:266-267`). O novo estado não copia sua regra; identifica propriedade temporal de uma P (`materializacao.py:1115-1126`). Porém a cauda adiciona veto antes de C7 (`disputa_emissao.py:1710-1712`); é decisão de ordem/ownership adicional e deve ser avaliada pelo aval literal, não apresentada como se viesse de C7.
- **JUIZES.md divergente:** diz “disputa fecha e SLA: SEM juiz” (`app/docs/JUIZES.md:54`), mas manda prevalecer o registro vivo (:10-14), onde C7 está registrado (`app/core/juizes.py:267`).
- **Segundo escritor fora da porta:** o diff não acrescenta escrita direta desses campos; a saída preservada passa por `p.carimbar` (`materializacao.py:1248-1249`). A guarda só protege os caminhos de S143 explicitamente cobertos (`disputa_emissao.py:1586`, :1671, :1710); proteção universal de toda chamada possível não vem dela (`materializacao.py:1395` não consulta o sinal).
- **Relógio fora da borda:** as novas funções usam só `ContextVar`, conjunto de pk e `in_atomic_block`, sem leitura de relógio (`materializacao.py:1103-1126`). As leituras de relógio próximas seguem nos carimbos/serviços já existentes (:1215; `disputa_emissao.py:1612`, :1669).
- **Derivação paralela:** não surge derivação nova de via ou lastro no trecho adicionado; o materializador continua lendo `declarou_hora`/`via_pelo_fato` (:1232-1249), e S143 continua usando C6/via pelo fato (`disputa_emissao.py:1592`, :1612, :1674, :1684).
- **Selo/RED sem mordida:** há mordida de escritor pela espionagem, mas os dois testes finais admitem sobrescrita; limites acima (`test_pep_reentrada.py:69-73`, :94-107). [nao verificado]
- **Ordem de trava:** a entrada fica atomic → ContextVar e a saída restaura token antes da saída de atomic (`materializacao.py:1191`, :1117-1121). Não aparece novo `select_for_update` nessas adições; comportamento concorrente real: [não verificado].
- **Pouso antes do pré-requisito:** carimbo final segue depois do plantio e da aprovação do juiz, ambos dentro do bloco (:1192-1210, :1248-1252); a cauda de fechamento regular segue depois do laço (:1346-1350). [nao verificado]
- **Flag global sobrevivente:** o objeto `ContextVar` é de módulo (:1103), mas o valor é instalado com token e restaurado no `finally` (:1117-1121); não é um booleano global de processo. Garantia de isolamento por thread/task e risco de contexto copiado requerem distinguir semântica do ContextVar de identidade de transação; execução desses contornos: [não verificado].

## 06/11 08:45 · sessao 4d47178d85d2 · - · - · origem=Ronald (tmux)

Escopo e aliases, por leitura estática:

- `_EM_VOO` é um objeto de módulo, mas o valor é um `frozenset` por contexto; o único escritor faz união com `p.pk` e restaura o token em `finally`. O fluxo normal não deixa o valor sobreviver ao próprio bloco. (`app/chamados/services/materializacao.py:1103`, `:1117`, `:1120`)
- Em `with atomic(), materializacao_em_voo(p)`, o marcador sai primeiro: o `reset` precede commit ou rollback do `atomic`. `_Reprovada` e exceções comuns são tratadas depois dessa saída; `return` interno também atravessa `finally`. (`app/chamados/services/materializacao.py:1191`, `:1199`, `:1210`, `:1254`, `:1256`)
- Rollback de savepoint interno não desfaz o marcador do bloco externo. Uma entrada aninhada restaura exatamente o contexto anterior: outra P acrescenta outro pk; a mesma P continua marcada quando o bloco interno sai. Isso não impede executar `_materializar_uma` duas vezes sobre a mesma P — é identificação, não trava de reentrada. (`app/chamados/services/materializacao.py:1117`, `:1121`, `:1129`)
- Callback normal de `on_commit` executado depois do bloco não verá a marca criada por esse bloco: ela já foi restaurada antes de `atomic.__exit__`. Há callbacks dessa forma nos receivers. (`app/chamados/services/materializacao.py:1191`, `:1121`; `app/chamados/signals.py:121`, `:314`)
- Outra thread/contexto independente não compartilha automaticamente o valor só porque o objeto está no módulo. A cópia configura `gthread`, quatro threads; implantação efetiva [não verificado]. (`app/chamados/services/materializacao.py:1103`; `app/gunicorn.conf.py:19`)
- A garantia universal “nada sobrevive” excede a implementação: um contexto explicitamente copiado ou uma tarefa que herde o contexto pode conservar o snapshot; o `reset` original restaura apenas o contexto original. Caminho concreto com `copy_context/create_task` na cópia: [não verificado]. O código armazena apenas pk, sem identidade de transação ou contexto filho. (`app/chamados/services/materializacao.py:1103`, `:1117`, `:1121`)
- Não é uma abstração segura para múltiplos aliases: `_em_atomic` consulta exclusivamente `django.db.connection`, e tanto o materializador como o receiver S143 abrem `atomic()` sem `using`. O receiver aceita `**kwargs`, mas não encaminha o alias do signal. (`app/chamados/services/materializacao.py:1106`, `:1191`; `app/chamados/signals.py:76`, `:91`)
- Os perfis SaaS e prod mostrados declaram somente `default`; SaaS usa `TenantSyncRouter` com backend de schemas. Não identifiquei chamador de S143 selecionando outra conexão; configuração efetiva de produção [não verificado]. (`app/config/settings/saas.py:83`, `:93`; `app/config/settings/prod.py:9`)
- Além do receiver, chamam S143 `_retratar`, `reatar_pergunta_orfa`, `fechar_beco` e o comando `reconciliar_fantasmas`, todos sem alias na chamada. (`app/chamados/reconciliador.py:536`, `:1631`; `app/chamados/services/becos.py:49`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`)
- O import local novo não introduz a dependência: `disputa_emissao` já importa `materializacao` no topo. O inverso existe como import local de `reabrir_para_correcao`; `models` reexporta primeiro `materializacao`, depois `disputa_emissao`. O import novo ocorre durante a função, depois da carga usual dos módulos, sem evidência estática de novo ciclo de inicialização. (`app/chamados/services/disputa_emissao.py:7`, `:1578`; `app/chamados/services/materializacao.py:642`; `app/chamados/models.py:3276`, `:3281`)
- `pergunta_em_voo` e seu import não consultam banco. A cauda nova, porém, avalia `d.perguntas.all()` para cada candidata aberta, mesmo quando `_EM_VOO` está vazio: acrescenta consulta/carregamento das perguntas nessa posição; custo medido [não verificado]. (`app/chamados/services/materializacao.py:1126`; `app/chamados/services/disputa_emissao.py:1710`)

## 06/11 08:45 · sessao 183ceb44a177 · LASTRO-PARTE-3 · verificador · origem=Code

**Há uma lacuna procedente, baixa: retirar a chamada de varredura de `_ancoradas` do selo deixa as cinco verificações verdes. A costura testa o scanner isoladamente, sem garantir sua inclusão no selo.** `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:252`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:283`.

1. **Kwargs alheios: possível; limitação procedente, baixa. Contaminação atual: não procedente.** O scanner percorre qualquer `ast.Call` e aceita qualquer kwarg cujo primeiro segmento coincida com campo da pergunta; não verifica método nem modelo. Assim, uma chamada hipotética `OutroModelo.objects.filter(recusa_motivo='x')` dentro da função seria contabilizada. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:233`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:235`.

   Na `_ancoradas` atual, todos os kwargs encontrados pertencem ao filtro de `PerguntaDisputa` ou aos seus `Q`. `select_related` e `order_by` recebem argumentos posicionais, que esse scanner não coleta. `app/chamados/models.py:327`; `app/chamados/models.py:328`; `app/chamados/models.py:329`; `app/chamados/models.py:330`.

   Essa sobreinclusão pode inflar `lidos`, mas não esconder campo ausente no teste principal: acrescentar elementos não elimina uma diferença já existente em `lidos - CAMPOS_QUE_ACORDAM`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`.

2. **Fecho FK escondendo um lado ausente: não procedente; sem severidade.** O fechamento amplia apenas os campos exigidos, não `CAMPOS_QUE_ACORDAM`. Para uma FK detectada, exige ambos os nomes; se faltar qualquer lado no conjunto produtivo, a diferença permanece não vazia. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:242`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:245`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`.

   Caso concreto: a leitura de `celula` exige também `celula_id`; retirar somente `celula_id` do conjunto reprova o selo principal. Hoje ambos estão presentes. `app/chamados/juizes.py:1147`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:219`; `app/chamados/models.py:2662`.

3. **Mordida das costuras: parcial; lacuna procedente, baixa.**

   | Mutante | Resultado das cinco verificações do selo |
   |---|---|
   | Retirar `celula_id` de `CAMPOS_QUE_ACORDAM` | O teste principal falha; a costura FK continua passando, pois sua própria expressão já subtrai esse campo. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:278`. |
   | Desativar o fechamento FK | A costura FK falha no `assertIn('celula_id', lidos)`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:277`. |
   | Fazer o scanner de kwargs retornar vazio | A costura do filtro falha. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:284`. |
   | Retirar somente a chamada do scanner de `_lidos_pelo_c6` | **Todas passam:** a costura chama o helper diretamente, e os campos atuais do filtro já aparecem nas outras funções. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:251`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:283`; `app/chamados/models.py:344`; `app/chamados/juizes.py:1136`. |

   Esses resultados foram reproduzidos em memória com os métodos extraídos da cópia e metadados substitutos derivados das declarações dos campos; execução integrada com Django: **[nao verificado]**. Código exercitado: `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:188`.

4. **Campo funcional direto faltante: não procedente; sem severidade.** As leituras diretas, filtros e pares FK resultam nos mesmos 12 nomes presentes em `CAMPOS_QUE_ACORDAM`: os seis campos escalares e os pares `celula`, `disputa` e `chamado_destino`. `app/chamados/juizes.py:1135`; `app/chamados/models.py:327`; `app/chamados/models.py:344`; `app/chamados/models.py:2661`; `app/chamados/models.py:2662`.

   Há duas ressalvas de escopo: `_ancoradas` usa `pk` para ordenação, fora do conjunto; e as leituras transitivas de `motivo`/`tipo_resposta` continuam fora, como declarado. A ordenação não acrescenta condição de seleção; a exclusão dos helpers permanece explícita, sem nova cura neste delta. `app/chamados/models.py:330`; `app/chamados/models.py:2660`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:195`; `app/chamados/juizes.py:886`; `app/chamados/juizes.py:1099`.

_2159 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
