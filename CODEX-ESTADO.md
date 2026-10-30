# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 20:17** · publicado em **30/10/2026 20:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 20:17 · sessao a717d30b276f · O261-E-D3 · censo · origem=Code

O outbox encontrado é **depois da lavra da célula**: registra alterações de `veredito`/`ata` e alimenta a reconciliação de chamados e perguntas. O rejulgamento reativo do cartório ocorre por chamadas síncronas, principalmente nos signals de insumos. (`app/ponto/portas/celula.py:622`, `app/chamados/reconciliador.py:1300`, `app/chamados/reconciliador.py:1303`, `app/ponto/signals.py:85`)

**1. Definição, chamadas e composição de `impressao_insumos`**

A definição está em `app/ponto/services/cartorio.py:314`. Há duas chamadas de produção, ambas em `_julgar_colab_corpo`: ramo sem adesão, em `app/ponto/services/cartorio.py:941`, e ramo normal, em `app/ponto/services/cartorio.py:1083`. `julgar_colab` chama esse corpo e pode repeti-lo para reconciliação/releitura. (`app/ponto/services/cartorio.py:744`, `app/ponto/services/cartorio.py:746`, `app/ponto/services/cartorio.py:811`)

O comando `processar_cartorio` **reexporta** a função, mas chama `julgar_colab`: busca colaboradores com células passadas, carrega suas células ordenadas por data e entrega a lista ao serviço. A porta unitária `julgar_celula` busca uma célula e chama o mesmo serviço com `[cel]`. (`app/ponto/management/commands/processar_cartorio.py:20`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:61`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1547`, `app/ponto/services/cartorio.py:1560`)

O resultado é `sha256('|'.join(partes).encode()).hexdigest()[:64]`; as partes são construídas na ordem abaixo. (`app/ponto/services/cartorio.py:323`, `app/ponto/services/cartorio.py:380`)

| Argumento | O que efetivamente entra no hash | Origem no chamador normal |
|---|---|---|
| `bs_janela` | Para cada batida: **`pk`, `tipo`, `timestamp.isoformat()`**, separados por `:`; batidas separadas por vírgula. (`app/ponto/services/cartorio.py:324`) | `bj`, recorte por data local da lista `bs`, carregada por `batidas_apuraveis`. (`app/ponto/services/cartorio.py:871`, `app/ponto/services/cartorio.py:1042`) |
| `cob_status` | Apenas `str(cob_status)`, inclusive `"None"` quando não há cobertura. (`app/ponto/services/cartorio.py:325`) | `getattr(cob, 'status', None)`, com `cob = ausencia_cobre(c, data, para='cobranca')`. (`app/ponto/services/cartorio.py:1041`, `app/ponto/services/cartorio.py:1083`) |
| `chamados` | Para cada chamado: **`pk`, `status_local`**; nenhum outro campo diretamente. (`app/ponto/services/cartorio.py:326`) | `lista = chs.get(data.isoformat(), [])`; `chs` vem de `chamados_que_falam_de_batida(c)`. (`app/ponto/services/cartorio.py:1004`, `app/ponto/services/cartorio.py:1040`) |
| `dna` | Somente **`str(dna_versao) + str(marcos)`**. Não é o JSON inteiro. (`app/ponto/services/cartorio.py:327`) | `cel.dna`, da célula recebida pelo serviço. (`app/ponto/services/cartorio.py:1083`) |
| `veto` | `veto:<valor>`, somente quando o valor é verdadeiro. (`app/ponto/services/cartorio.py:329`) | `_veto_do_dia(data)` → `motivo_do_veto(c, vinculo_do_emissor(c,data),data)`. (`app/ponto/services/cartorio.py:1011`, `app/ponto/services/cartorio.py:1013`) |
| `teto` | `teto:<valor>`, quando diferente de `None`. (`app/ponto/services/cartorio.py:334`) | Quantidade de marcos vencidos retornados por `marcos_vencidos`, nos caminhos de `teto_de_hoje`; normalmente `None`. (`app/ponto/services/cartorio.py:1048`, `app/ponto/services/cartorio.py:1053`, `app/ponto/services/cartorio.py:1067`, `app/ponto/services/cartorio.py:1085`) |
| `regra` | `regra:<valor>`, quando diferente de `None`. (`app/ponto/services/cartorio.py:342`) | `_regra_do_dia(data)` → `versao_para(data,c.empresa)`. A versão vem de `REGRA_VERSOES`, escolhida pela competência de `janela_atual`. (`app/ponto/services/cartorio.py:100`, `app/ponto/services/cartorio.py:183`, `app/ponto/services/cartorio.py:888`) |
| `fio` | `fio:<valor>`, quando diferente de `None`; **zero entra**. (`app/ponto/services/cartorio.py:367`) | Número de chamados vivos da lista que possuem pergunta viva: `_com_fio`. Sem chamados vivos, recebe `None`. (`app/ponto/services/cartorio.py:1069`, `app/ponto/services/cartorio.py:1074`, `app/ponto/services/cartorio.py:1087`) |
| `cob_parcial` | Quando verdadeiro: `cobparcial:1`; sendo string, acrescenta `:<estado>`. (`app/ponto/services/cartorio.py:375`) | `dia['periodo_parcial']` ou o booleano `dia['cobertura_parcial']`, lidos da grade. (`app/ponto/services/cartorio.py:1079`, `app/ponto/services/cartorio.py:1088`) |
| `enc` | `enc:<valor>`, quando diferente de `None`. (`app/ponto/services/cartorio.py:352`) | `0` para dia passado cujo turno permanece em curso e não está sendo julgado até um teto; caso contrário, `None`. (`app/ponto/services/cartorio.py:1082`, `app/ponto/services/cartorio.py:1084`) |

Detalhes da alimentação desses argumentos:

- `chamados_que_falam_de_batida` consulta `ChamadoColaborador` do colaborador, filtra `modulo_origem__in=FURO_MODULES`, faz prefetch de `perguntas_que_resolvem` e agrupa pela data obtida por `data_do_chamado`. Os módulos são `batida_ausente`, `turno_aberto`, `intervalo_pendente` e `entrada_bloqueada_turno_aberto`. (`app/ponto/services/cartorio.py:85`, `app/ponto/services/cartorio.py:93`, `app/ponto/services/cartorio.py:96`, `app/chamados/catalogo/furo_modulos.py:15`)
- `ausencia_cobre` consulta `Ausencia`, exclui rejeitadas para cobrança, considera dia único/range fechado/range aberto permitido e escolhe a vencedora por `-data,-pk`. (`app/ponto/turnos.py:2489`, `app/ponto/turnos.py:2525`, `app/ponto/turnos.py:2530`)
- A grade escreve `cobertura_parcial` usando `cobre_parte_do_dia` e escreve `periodo_parcial` usando `periodo_da_parcial_no_dia`. A parcialidade depende de tipo habilitado para horas, dia único e `0 < minutos < 480`; o estado do período pode ser `sem_periodo`, `dentro`, `fora` ou `nao_sei`. (`app/escala/utils.py:1147`, `app/escala/utils.py:1190`, `app/escala/utils.py:1194`, `app/ponto/turnos.py:2571`, `app/ponto/turnos.py:2587`)
- `fio_vivo` reduz as perguntas recebidas a `any(pergunta_viva(p) ...)`. (`app/chamados/juizes.py:1102`, `app/chamados/juizes.py:1111`)
- `marcos_vencidos` obtém o vínculo por `escala_vigente`, lê `marcos_turno` e compara cada instante com `agora`, acrescentando a margem de 30 minutos. (`app/ponto/services/cartorio.py:662`, `app/ponto/services/cartorio.py:675`, `app/ponto/services/cartorio.py:679`)
- `_turno_encerrado` vem de `dias_encerrados(c,d0,d1,agora=...,celulas=...)`. (`app/ponto/services/cartorio.py:850`)

No DNA produzido hoje, `marcos` contém `hi`, `hf`, `hii`, `hfi` e, quando houver segunda pausa, `hii2`, `hfi2`; as horas são convertidas para `HH:MM`. Como entra `str(marcos)`, entra a representação do dicionário inteiro nessa parcela. **Ficam fora dessa parcela** `vinculo_id`, `tipo_escala_id`, `tipo_ciclo`, `minutos_jornada`, `intervalo_livre`, `lampadas` e `extra_declarada`. (`app/ponto/management/commands/gerar_celulas.py:32`, `app/ponto/management/commands/gerar_celulas.py:83`, `app/ponto/management/commands/gerar_celulas.py:92`, `app/ponto/management/commands/gerar_celulas.py:95`, `app/ponto/management/commands/gerar_celulas.py:121`, `app/ponto/services/cartorio.py:327`)

No ramo **`adesao is None`**, a chamada passa `bs_janela=[]`, `chamados=[]`, a cobertura obtida por `ausencia_cobre`, `cel.dna`, `regra` e a parcialidade da grade. `veto`, `teto`, `fio` e `enc` permanecem nos defaults `None`. A adesão é obtida pela primeira batida não retratada da vida do colaborador. (`app/ponto/services/cartorio.py:899`, `app/ponto/services/cartorio.py:907`, `app/ponto/services/cartorio.py:924`, `app/ponto/services/cartorio.py:933`, `app/ponto/services/cartorio.py:941`)

**Janela de batidas:** para a célula de **D**, o hash normal recebe batidas de **D−1, D e D+1**, pela data local — intervalo equivalente a `[D−1 00:00, D+2 00:00)`. A carga geral do lote é maior: `[d0−1 00:00, d1+3 00:00)`, depois recortada por célula. São excluídas batidas retratadas, e a ordenação é por `timestamp`. (`app/ponto/services/cartorio.py:855`, `app/ponto/services/cartorio.py:856`, `app/ponto/services/cartorio.py:871`, `app/ponto/services/cartorio.py:1042`, `app/ponto/turnos.py:2845`, `app/ponto/turnos.py:2852`)

Também há chamadas diretas em testes: [nao verificado]

| Arquivo | Linhas das chamadas |
|---|---|
| `app/ponto/tests/test_cartorio.py` | 16, 17, 22, 23, 29, 30, 34, 35 |
| `app/ponto/tests/test_veto_que_cai_rejulga.py` | 136, 137, 138, 141 |
| `app/ponto/tests/test_vigia_de_hora.py` | 144, 145 |
| `app/ponto/tests/test_o219_versao_da_regra.py` | 97, 102, 110, 111, 112, 118, 119, 146 |
| `app/ponto/tests/test_selo_b5u_nunca_bateu_ve_ausencia.py` | 40, 41, 47, 48, 129 |
| `app/ponto/tests/test_o146_extra_declarada_da_escala.py` | 423, 424 |
| `app/ponto/tests/test_o261_cobrado_le_o_fio.py` | 209, 210, 211, 212 |
| `app/ponto/tests/test_celula_concorde_meio_turno.py` | 56, 57, 86, 87, 88 |
| `app/escala/tests/test_cobertura_parcial_periodo.py` | 203, 204, 205, 206, 207 |

**2. Os nove insumos: hash, portas e disparos**

Primeiro, o mecanismo comum:

- `EventoCelula` é o outbox encontrado; contém identificadores, data, criação, processamento, tentativas e erro. `celula_id` é inteiro, **sem FK**. (`app/escala/models.py:1800`, `app/escala/models.py:1808`)
- `salvar_e_emitir` salva a célula e insere evento no mesmo `atomic` quando os campos lavrados incluem `veredito` ou `ata`. Alterar apenas DNA ou campos de insumos não satisfaz esse critério. (`app/ponto/portas/celula.py:623`, `app/ponto/portas/celula.py:634`, `app/ponto/portas/celula.py:638`)
- O consumidor lê o evento e chama `retratar_chamados_absolvidos_por_celula` e `encerrar_orfas_pela_lampada`. Uma alteração de status de chamado feita nessa reconciliação pode, por sua vez, chamar o cartório pelo signal do chamado. (`app/chamados/reconciliador.py:1300`, `app/chamados/reconciliador.py:1303`, `app/chamados/signals.py:164`, `app/chamados/signals.py:180`)
- O consumidor está configurado a cada minuto; o cartório periódico roda com `--apply`, sem `--forcar`. A “fila” deste último é o queryset de células passadas, e o hash igual permite pular o julgamento. (`app/config/crons.py:375`, `app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/services/cartorio.py:1092`)
- `out['rejulgar']` é uma lista em memória para segunda passada, com descarte contado das sobras; não é outbox persistente. (`app/ponto/services/cartorio.py:745`, `app/ponto/services/cartorio.py:751`)
- Não localizei outra tabela/função de fila persistente para rejulgar `CelulaDia`, pesquisando `Outbox`, `Pendencia`, `Fila`, `enfileirar`, `agendar`, `rejulgar`, `julgar_celula` e `julgar_colab` nos modelos, signals, services e comandos de `app/`. Infraestrutura externa ao código disponível: [nao verificado].

**(i) Batidas do dia**

**Hash:** entram diretamente por `bs_janela`, apenas `pk/tipo/timestamp`. `retratada_em` atua indiretamente, retirando ou recolocando a batida no universo apurável; `aprovada` isoladamente não é filtro desse universo. (`app/ponto/services/cartorio.py:324`, `app/ponto/turnos.py:2845`)

**Portas:** criação por `registrar_batida`; alteração por `anotar`; retratação por `retratar_batida`; exclusão por `apagar`. `flip_tipo` usa `anotar`; retratação/restauração humana usa `retratar_humano.retratar/desfazer`. (`app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:235`, `app/ponto/registro_batida.py:267`, `app/ponto/registro_batida.py:285`, `app/ponto/services/flip_batida.py:30`, `app/ponto/services/retratar_humano.py:179`, `app/ponto/services/retratar_humano.py:207`)

Entradas que chegam à criação incluem API/PWA, materialização de justificativa, resposta de pergunta, regularização externa e decisão do dia. (`app/api/views.py:683`, `app/api/views_core.py:680`, `app/ponto/services/triagem_batida.py:708`, `app/ponto/services/justificativa.py:173`, `app/ponto/services/justificativa.py:346`, `app/chamados/services/materializacao.py:872`, `app/chamados/services/materializacao.py:952`, `app/chamados/services/regularizacao_ext.py:77`, `app/ponto/services/veredito_celula.py:301`)

**Disparo:** save/delete chama sincronicamente `julgar_celula` para a data local da batida e sua véspera. Não enfileira julgamento em outbox; a lavra resultante pode produzir o evento posterior. (`app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/signals.py:138`, `app/ponto/signals.py:93`, `app/ponto/portas/celula.py:638`)

A interface de importação AEJ retorna HTTP 410. O bulk de `semear` exige banco de ensaio e recusa o banco de produção indicado no código. (`app/ponto/views.py:57`, `app/ponto/views.py:70`, `app/ponto/registro_batida.py:326`, `app/ponto/registro_batida.py:331`)

**(ii) Batidas do dia seguinte**

**Hash e portas:** são as mesmas batidas/portas do item anterior; D+1 faz parte expressamente do recorte do hash de D. (`app/ponto/services/cartorio.py:1042`, `app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:235`)

**Disparo:** uma batida de D+1 aciona D pelo signal que julga a véspera. Há uma assimetria: uma batida de D−1 também pertence ao hash de D, mas seu signal julga D−2/D−1, **não D**. (`app/ponto/signals.py:108`, `app/ponto/signals.py:112`, `app/ponto/services/cartorio.py:1042`)

**(iii) `CelulaDia.dna`**

**Hash:** diretamente, mas **parcialmente**: somente `dna_versao` e `marcos`; modificar outra chave isoladamente não muda essa parcela. (`app/ponto/services/cartorio.py:327`)

**Portas e disparos:**

| Porta | Escrita e julgamento |
|---|---|
| `gerar_celulas_periodo` | Cria, completa DNA nulo passado e atualiza células presentes/futuras; termina sem chamada ao cartório/outbox. (`app/ponto/portas/celula.py:593`, `app/ponto/portas/celula.py:603`, `app/ponto/portas/celula.py:610`, `app/ponto/portas/celula.py:617`) |
| `regenerar_celulas_vinculo` | Reescreve DNA; com `recalculo=True`, força julgamento dos dias tocados até hoje, excetuando vereditos humanos. (`app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:365`, `app/ponto/portas/celula.py:372`) |
| `regenerar_celulas_dia` | Delega ao regenerador de vínculo com `[dia,dia]` e sem extensão do horizonte. (`app/ponto/portas/celula.py:474`) |
| `reverter_regeneracao_dia` | Repõe `dna_anterior`, salva e registra trilha; termina sem julgamento/outbox. (`app/ponto/portas/celula.py:530`, `app/ponto/portas/celula.py:538`, `app/ponto/portas/celula.py:552`) |

Não há emissão direta do outbox apenas pela escrita de DNA: o critério é `veredito/ata`. Não localizei receiver de `CelulaDia` nos signals de `app/`; a ausência de signal nessa cadeia também está declarada no código. (`app/ponto/portas/celula.py:623`, `app/ponto/portas/celula.py:332`; busca negativa: [nao verificado])

**(iv) Vínculo vigente / escala**

**Hash:** o vínculo não é serializado. Pode entrar indiretamente pelos marcos do DNA regenerado, pelo `veto` e pelos valores derivados `teto/enc`. O julgamento ainda recebe cadastro vivo do vínculo, carregado separadamente, sem serializá-lo integralmente no hash. (`app/ponto/services/cartorio.py:327`, `app/ponto/services/cartorio.py:839`, `app/ponto/services/cartorio.py:1013`, `app/ponto/services/cartorio.py:1083`, `app/ponto/services/cartorio.py:1118`)

O escritor declarado de `EscalaColaborador` é `colaboradores/services/vinculo.py`. (`app/core/portas.py:465`)

| Portas/caminho | Disparo efetivo |
|---|---|
| `executar_vinculo` | Cria/atualiza vínculo, regenera células e chama `reconciliar_apos_vinculo`, que força julgamento, com exclusões de dias humanos/já julgados. (`app/colaboradores/services/vinculo.py:265`, `app/colaboradores/services/vinculo.py:278`, `app/colaboradores/services/vinculo.py:320`, `app/colaboradores/services/vinculo.py:328`, `app/colaboradores/services/pos_vinculo.py:50`, `app/colaboradores/services/pos_vinculo.py:72`) |
| `encerrar_vinculo`, `corrigir_vinculo_retroativo` | Usam save; mudanças de vigência/âncora acionam regeneração pelos signals correspondentes. (`app/colaboradores/services/vinculo.py:799`, `app/colaboradores/services/vinculo.py:845`, `app/escala/signals.py:71`, `app/escala/signals.py:117`) |
| `fechar_vigencia`, `reabrir_vigencia_impossivel`, `absorver_vigencias_posteriores`, `invalidar_ancoras_incoerentes`, `reverter_para_snapshot` | Fazem `update()` sem o signal de save; esses corpos não chamam cartório/outbox. O chamador pode complementar o ato. (`app/colaboradores/services/vinculo.py:615`, `app/colaboradores/services/vinculo.py:691`, `app/colaboradores/services/vinculo.py:753`, `app/colaboradores/services/vinculo.py:857`, `app/colaboradores/services/vinculo.py:874`) |
| `trocar_calendario_de_folga`, `marcar_trabalha_em_feriado` | Salvam essas chaves, que não são vigência/âncora; não há rejulgamento direto nesses corpos. (`app/colaboradores/services/vinculo.py:445`, `app/colaboradores/services/vinculo.py:825`, `app/escala/signals.py:57`, `app/escala/signals.py:107`) |
| `criar_trecho_retroativo`, `ajustar_vinculo_pelo_sistema`, `desfazer_ajuste_do_sistema` | Criam/fecham/restauram vínculos; os receivers de regeneração pulam `created`, e há atualizações sem signal nesses caminhos. (`app/colaboradores/services/vinculo.py:832`, `app/colaboradores/services/vinculo.py:928`, `app/colaboradores/services/vinculo.py:940`, `app/escala/signals.py:54`, `app/escala/signals.py:104`) |
| `salvar_tipo` → `_propagar_regeneracao_template` | Regenera com **`recalculo=False`**; portanto não executa o julgamento da regeneração. Também invalida o fechamento. (`app/escala/services/cadastro_tipo.py:320`, `app/escala/services/cadastro_tipo.py:348`, `app/escala/services/cadastro_tipo.py:350`, `app/escala/services/cadastro_tipo.py:357`, `app/ponto/portas/celula.py:360`) |

Folgas também alteram esse desenho: `lancar_folga`, `alterar_folga`, `aplicar_folgas_em_lote` e `gerar_foto_folgas` chamam `relavrar_celulas_da_foto`, que regenera com o default de julgamento ligado. Salvar o calendário isoladamente apenas grava o cadastro. (`app/escala/folgas.py:192`, `app/escala/folgas.py:220`, `app/escala/folgas.py:261`, `app/escala/folgas.py:303`, `app/escala/folgas.py:340`, `app/escala/folgas.py:468`)

A régua **de defesa do vínculo**, distinta da CCT, é gravada por `gravar_regua` com update sem signal. `desfazer_corroboracao` possui chamada explícita a `rejulgar_furos_do_vinculo`, que usa `julgar_colab`. (`app/colaboradores/services/vinculo.py:889`, `app/colaboradores/services/vinculo.py:911`, `app/escala/regua_defesa.py:719`, `app/escala/regua_defesa.py:725`, `app/escala/regua_defesa.py:692`)

Nenhum desses caminhos é enqueue de julgamento em outbox; os que julgam chamam o cartório diretamente, e a eventual lavra pode emitir `EventoCelula`. (`app/ponto/portas/celula.py:372`, `app/ponto/portas/celula.py:638`)

**(v) Ausências/coberturas: férias, atestado, afastamento, abono**

**Hash:** entra o **status da ausência vencedora** por `cob_status`, mais a classificação de parcialidade/período por `cob_parcial`. Não entram integralmente `pk`, tipo, datas, minutos ou horários da ausência. Datas e tipo influenciam a seleção da cobertura; tipo/minutos/horários podem influenciar a classificação parcial. Duas ausências diferentes podem produzir os mesmos componentes de hash. (`app/ponto/services/cartorio.py:325`, `app/ponto/services/cartorio.py:375`, `app/ponto/services/cartorio.py:1088`, `app/ponto/turnos.py:2525`, `app/ponto/turnos.py:2571`, `app/ponto/turnos.py:2605`)

**Portas:** `criar_ausencia`/`garantir_ausencia`; `editar_ausencia_registro`; `mudar_estado`/`decidir`; `anexar_documento`; `reverter_rejeicao`/`_reabrir`; `rederivar_fim`. A declaração/edição do período parcial passa por `decidir` → `_aplicar_periodo`. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:453`, `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:792`, `app/ponto/services/aus

[truncado em 20000 de 39882 caracteres]

## 30/10 20:14 · sessao ab5275193aec · - · - · origem=Ronald (tmux)

- **`ferias.Afastamento` é outra entidade:** cadastro de estado com início/retorno, editável pelo `AfastamentoAdmin`; não possui sync para Ausencia no modelo. O julgamento operacional consulta **`ponto.Ausencia`, tipo `afastamento_inss`**, por `afastamentos_no_dia`, não `ferias.Afastamento`. Não achei receiver de Afastamento nos `app/*/signals.py`, nem chamada cartório/outbox nesse admin/modelo. (`app/ferias/models.py:488`, `app/ferias/models.py:499`, `app/ferias/admin.py:4`, `app/ferias/admin.py:10`, `app/ponto/turnos.py:2683`, `app/ponto/turnos.py:2693`, `app/ponto/turnos.py:2694`; busca negativa nos arquivos citados).

- **Cobertura por substituição não é `cob_status`:** `designar_cobertura` cria `OrdemSubstituicao`; `declarar_cobertura_passada` cria ordem concluída, quando há substituto. Não escrevem DNA/agenda/Ausencia e a ordem não é serializada no hash. Podem mudar hash **via status dos chamados**, pois tentam `ch.reconciliar()`; mudança de status dispara o cartório por signal. O gate pode recusar o fechamento porque a célula/ata não lê essa cobertura. Não há signal de OrdemSubstituicao nos `app/*/signals.py`, nem enqueue direto de rejulgamento/outbox nessas portas. (`app/ponto/services/cobertura.py:22`, `app/ponto/services/cobertura.py:100`, `app/ponto/services/cobertura.py:122`, `app/ponto/services/cobertura.py:246`, `app/ponto/services/cobertura.py:289`, `app/ponto/services/cobertura.py:315`, `app/ponto/services/cobertura.py:318`, `app/ponto/services/cartorio.py:325`, `app/ponto/services/cartorio.py:326`, `app/chamados/signals.py:164`, `app/chamados/signals.py:180`; busca negativa nos signals).

- **Folgas têm gatilho efetivo de regeneração/julgamento:** `lancar_folga`, `alterar_folga`, `aplicar_folgas_em_lote` e `gerar_foto_folgas` chamam `relavrar_celulas_da_foto`, que chama `regenerar_celulas_vinculo` com o default `recalculo=True`; esta força `julgar_celula` dos dias tocados até hoje, exceto humanos. Entram indiretamente no hash pelos marcos do DNA que a regeneração altera, sem enqueue direto. (`app/escala/folgas.py:192`, `app/escala/folgas.py:198`, `app/escala/folgas.py:220`, `app/escala/folgas.py:228`, `app/escala/folgas.py:261`, `app/escala/folgas.py:293`, `app/escala/folgas.py:303`, `app/escala/folgas.py:308`, `app/escala/folgas.py:340`, `app/ponto/portas/celula.py:102`, `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:372`, `app/ponto/services/cartorio.py:327`).

- **Salvar calendário isoladamente não relavra:** `salvar_calendario_de_folga` apenas grava calendário/tamanhos. Já as portas que o **aplicam ao vínculo** chamam `gerar_foto_folgas` e chegam à cadeia acima: `vincular_folga` e aplicação em lote do tipo. (`app/escala/folgas.py:468`, `app/escala/folgas.py:479`, `app/escala/folgas.py:490`, `app/colaboradores/views.py:212`, `app/colaboradores/views.py:239`, `app/colaboradores/views.py:245`, `app/escala/views.py:412`, `app/escala/views.py:414`, `app/escala/views.py:419`).

## 30/10 20:13 · sessao ab5275193aec · - · - · origem=Ronald (tmux)

Consolidação dos insumos sob minha exploração, somente leitura:

**Mecanismo de rejulgamento e outbox**

- Os receivers de Batida e Ausencia chamam diretamente `cartorio.julgar_celula`, pelo helper `_julgar_dias`; não enfileiram julgamento. O helper evita reentrada quando `cartorio.em_julgamento()` é verdadeiro. (`app/ponto/signals.py:85`, `app/ponto/signals.py:90`, `app/ponto/signals.py:93`, `app/ponto/signals.py:131`, `app/ponto/signals.py:143`).
- O outbox encontrado é **posterior à lavra**: `EventoCelula` registra alterações de veredito/ata; `salvar_e_emitir` salva e cria o evento no mesmo `atomic`. (`app/escala/models.py:1800`, `app/ponto/portas/celula.py:623`, `app/ponto/portas/celula.py:626`, `app/ponto/portas/celula.py:634`).
- Seu consumidor chama `retratar_chamados_absolvidos_por_celula` e `encerrar_orfas_pela_lampada`, sem chamar o cartório. Portanto, esse outbox não é uma fila de rejulgamento de `CelulaDia`. (`app/chamados/reconciliador.py:1257`, `app/chamados/reconciliador.py:1300`, `app/chamados/reconciliador.py:1303`).
- A “fila” do comando `processar_cartorio` é uma consulta a colaboradores com células passadas; o comando carrega todas as células passadas e chama `julgar_colab`. (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:61`, `app/ponto/management/commands/processar_cartorio.py:65`).
- Busca negativa: procurei `outbox|Outbox|Fila|Pendencia|enfileirar|agendar|rejulgar` nos modelos, signals e services de `app/`; não encontrei tabela/função que enfileire rejulgamento da célula. O outbox encontrado tem o comportamento acima; o comando tem a consulta acima. [não verificado quanto a infraestrutura externa ao repositório].

**(i) Batidas do dia e (ii) do seguinte**

- Entram diretamente por `bs_janela`: para cada batida, `pk`, `tipo`, `timestamp.isoformat()`, unidos por vírgulas. Não entram diretamente `aprovada`, `origem`, foto, justificativa, GPS ou `retratada_em`. (`app/ponto/services/cartorio.py:324`).
- O ramo normal seleciona batidas com data local **D−1, D e D+1**, inclusivamente; a carga é ordenada por timestamp e exclui retratadas. (`app/ponto/services/cartorio.py:1042`, `app/ponto/services/cartorio.py:871`, `app/ponto/turnos.py:2845`, `app/ponto/turnos.py:2852`).
- `retratada_em` entra indiretamente pela inclusão/exclusão da batida nessa lista. `aprovada=False` isoladamente não exclui a batida, pois o filtro de apuração é `retratada_em__isnull=True`. (`app/ponto/turnos.py:2845`).
- Portas centrais: `registrar_batida` cria; `anotar` muda campos existentes; `retratar_batida` chama `anotar` com `aprovada=False` e `retratada_em=now`; `apagar` faz delete. (`app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:235`, `app/ponto/registro_batida.py:254`, `app/ponto/registro_batida.py:267`, `app/ponto/registro_batida.py:274`, `app/ponto/registro_batida.py:285`, `app/ponto/registro_batida.py:291`).
- A porta de anotação permite `tipo`, aprovação/autoria/data, origem, retratação, foto, justificativa e pergunta de origem; não permite alterar timestamp. (`app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:241`).
- `flip_tipo` altera E↔S usando `anotar`; `retratar_humano.retratar` usa `retratar_batida`; `retratar_humano.desfazer` restaura `aprovada=True,retratada_em=None` usando `anotar`. (`app/ponto/services/flip_batida.py:10`, `app/ponto/services/flip_batida.py:30`, `app/ponto/services/retratar_humano.py:150`, `app/ponto/services/retratar_humano.py:179`, `app/ponto/services/retratar_humano.py:190`, `app/ponto/services/retratar_humano.py:207`).
- As views de retratar/desfazer são `retratar_batida_view` e `desfazer_retratacao_view`; ambas chamam esses services. (`app/ponto/views.py:3264`, `app/ponto/views.py:3270`, `app/ponto/views.py:3276`, `app/ponto/views.py:3282`).
- Criadores de produção via `registrar_batida`: API `api_bater_ponto`; PWA `ramo_registrar_es`; aprovação/materialização de justificativa; materialização de resposta hora e data/hora; regularização externa; Resolver dia; ações de chamado; lote de justificativas. (`app/api/views.py:400`, `app/api/views.py:683`, `app/api/views_core.py:387`, `app/api/views_core.py:680`, `app/ponto/services/triagem_batida.py:616`, `app/ponto/services/triagem_batida.py:708`, `app/ponto/services/justificativa.py:117`, `app/ponto/services/justificativa.py:173`, `app/ponto/services/justificativa.py:316`, `app/ponto/services/justificativa.py:346`, `app/chamados/services/materializacao.py:724`, `app/chamados/services/materializacao.py:872`, `app/chamados/services/materializacao.py:923`, `app/chamados/services/materializacao.py:952`, `app/chamados/services/regularizacao_ext.py:36`, `app/chamados/services/regularizacao_ext.py:77`, `app/ponto/services/veredito_celula.py:301`, `app/chamados/services/acoes_chamado.py:44`, `app/ponto/views.py:1764`, `app/ponto/views.py:1816`).
- Save/delete de batida julga **a véspera da data da batida e a própria data**. Assim, batida de D+1 dispara julgamento de D; batida de D−1, embora esteja no hash de D, não dispara D por esse receiver. (`app/ponto/signals.py:108`, `app/ponto/signals.py:112`, `app/ponto/signals.py:131`, `app/ponto/signals.py:135`, `app/ponto/signals.py:138`, `app/ponto/signals.py:140`).
- Essas portas não inserem diretamente no outbox; o julgamento disparado pode emitir `EventoCelula` se alterar veredito/ata. (`app/ponto/signals.py:93`, `app/ponto/portas/celula.py:638`, `app/ponto/portas/celula.py:642`).
- Importação AEJ via interface está desativada: `importar_aej` devolve HTTP 410; a URL ainda aponta para essa view; código legado está em `.disabled`. (`app/ponto/views.py:57`, `app/ponto/views.py:65`, `app/ponto/views.py:70`, `app/ponto/urls.py:9`, `app/ponto/legacy/aej_importer.py.disabled:9`).
- Único `bulk_create` de Batida encontrado nos `.py` de `app/` fora testes/migrations: `semear`, restrito a banco de ensaio e recusado sem `BANCO_DE_ENSAIO` ou se o banco se chama `saas_hasner`; não é importação de produção. (`app/ponto/registro_batida.py:316`, `app/ponto/registro_batida.py:326`, `app/ponto/registro_batida.py:331`, `app/ponto/registro_batida.py:335`). Existência de sincronização SQL externa: [não verificado]; o comentário da view apenas a menciona (`app/ponto/views.py:61`).

**(iv) Vínculo vigente/escala**

- O objeto vínculo não é serializado no hash. Do DNA entram somente `dna_versao` e `marcos`; `vinculo_id`, `tipo_escala_id`, `tipo_ciclo`, `minutos_jornada`, `intervalo_livre` e `lampadas` ficam fora dessa parcela. (`app/ponto/services/cartorio.py:327`, `app/ponto/management/commands/gerar_celulas.py:95`).
- Influência indireta: regeneração transforma o vínculo/template em DNA; alteração de marcos muda o hash. O veto também entra: `_veto_do_dia` resolve vínculo do emissor e `motivo_do_veto`; este testa régua degenerada/suspeita. (`app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:260`, `app/ponto/services/cartorio.py:1011`, `app/ponto/services/cartorio.py:1013`, `app/ponto/services/furos_vetados.py:18`, `app/ponto/services/furos_vetados.py:27`, `app/ponto/services/furos_vetados.py:40`, `app/ponto/services/furos_vetados.py:42`).
- O julgamento ainda lê dados vivos do vínculo via `cadastro_do_dia`, alimentado por todos os vínculos do colaborador e pelas células em julgamento; essa informação não é serializada integralmente no hash. (`app/ponto/services/cartorio.py:839`, `app/ponto/services/cartorio.py:842`, `app/ponto/services/cartorio.py:1118`).
- Módulo de escrita declarado para EscalaColaborador: `colaboradores/services/vinculo.py`. (`app/core/portas.py:465`).
- `executar_vinculo` cria/atualiza o vínculo, regenera células e chama `reconciliar_apos_vinculo`; esta força `julgar_celula` para os dias até hoje, excetuando vereditos humanos e dias já julgados pela regeneração. (`app/colaboradores/services/vinculo.py:167`, `app/colaboradores/services/vinculo.py:265`, `app/colaboradores/services/vinculo.py:274`, `app/colaboradores/services/vinculo.py:278`, `app/colaboradores/services/vinculo.py:320`, `app/colaboradores/services/vinculo.py:328`, `app/colaboradores/services/pos_vinculo.py:50`, `app/colaboradores/services/pos_vinculo.py:61`, `app/colaboradores/services/pos_vinculo.py:72`).
- `encerrar_vinculo` e `corrigir_vinculo_retroativo` usam save; receivers de alteração de vigência e âncora chamam regeneração. (`app/colaboradores/services/vinculo.py:780`, `app/colaboradores/services/vinculo.py:799`, `app/colaboradores/services/vinculo.py:839`, `app/colaboradores/services/vinculo.py:845`, `app/escala/signals.py:34`, `app/escala/signals.py:71`, `app/escala/signals.py:79`, `app/escala/signals.py:117`).
- A regeneração força julgamento dos dias efetivamente tocados até hoje, sem vereditos humanos, **apenas quando `recalculo=True`**. (`app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:365`, `app/ponto/portas/celula.py:369`, `app/ponto/portas/celula.py:372`).
- Alteração de template: `salvar_tipo` → `_propagar_regeneracao_template` → regeneração com `recalculo=False`; essa porta não força o rejulgamento anterior e não emite outbox só por mudar DNA. (`app/escala/services/cadastro_tipo.py:140`, `app/escala/services/cadastro_tipo.py:306`, `app/escala/services/cadastro_tipo.py:320`, `app/escala/services/cadastro_tipo.py:327`, `app/escala/services/cadastro_tipo.py:348`, `app/escala/services/cadastro_tipo.py:350`, `app/ponto/portas/celula.py:360`, `app/ponto/portas/celula.py:623`).
- Portas que fazem `update()` sem signal e não chamam cartório/outbox dentro de seus corpos: `fechar_vigencia`; `reabrir_vigencia_impossivel`; `absorver_vigencias_posteriores`; `invalidar_ancoras_incoerentes`; `reverter_para_snapshot`. Chamadores podem executar ações posteriores, como faz `executar_vinculo`. (`app/colaboradores/services/vinculo.py:561`, `app/colaboradores/services/vinculo.py:615`, `app/colaboradores/services/vinculo.py:635`, `app/colaboradores/services/vinculo.py:691`, `app/colaboradores/services/vinculo.py:710`, `app/colaboradores/services/vinculo.py:753`, `app/colaboradores/services/vinculo.py:849`, `app/colaboradores/services/vinculo.py:857`, `app/colaboradores/services/vinculo.py:860`, `app/colaboradores/services/vinculo.py:874`).
- Outras portas: `trocar_calendario_de_folga`, `marcar_trabalha_em_feriado`, `criar_trecho_retroativo`, `ajustar_vinculo_pelo_sistema`, `desfazer_ajuste_do_sistema`. As duas primeiras salvam chaves que não são vigência/âncora; os receivers de regeneração só detectam as mudanças que seus nomes indicam e pulam `created`. (`app/colaboradores/services/vinculo.py:433`, `app/colaboradores/services/vinculo.py:445`, `app/colaboradores/services/vinculo.py:822`, `app/colaboradores/services/vinculo.py:825`, `app/colaboradores/services/vinculo.py:829`, `app/colaboradores/services/vinculo.py:832`, `app/colaboradores/services/vinculo.py:925`, `app/colaboradores/services/vinculo.py:930`, `app/colaboradores/services/vinculo.py:936`, `app/colaboradores/services/vinculo.py:940`, `app/escala/signals.py:54`, `app/escala/signals.py:58`, `app/escala/signals.py:104`, `app/escala/signals.py:108`).
- O escore/corroborração da régua é escrito por `gravar_regua`, com update sem signal, e pode mudar o `veto` do hash. (`app/colaboradores/services/vinculo.py:889`, `app/colaboradores/services/vinculo.py:911`, `app/ponto/services/furos_vetados.py:42`).

**(v) Ausências/coberturas, inclusive férias**

- Entram pelo **status da ausência vencedora**, argumento `cob_status`; não entram diretamente pk, tipo, minutos ou datas. A vencedora vem de `ausencia_cobre(c,D,para='cobranca')`, que exclui rejeitadas, seleciona cobertura de dia/range aberto/fechado e desempata por `-data,-pk`. (`app/ponto/services/cartorio.py:325`, `app/ponto/services/cartorio.py:1041`, `app/ponto/services/cartorio.py:1083`, `app/ponto/turnos.py:2487`, `app/ponto/turnos.py:2490`, `app/ponto/turnos.py:2525`, `app/ponto/turnos.py:2530`).
- Datas/range entram indiretamente por determinar se há cobertura e qual vence; tipos/minutos/horários parciais podem alterar `cob_parcial`/`periodo_parcial`, lidos da grade e adicionados como `cobparcial:1[:estado]`. Isso não serializa os horários/minutos integralmente. (`app/ponto/services/cartorio.py:1079`, `app/ponto/services/cartorio.py:1088`, `app/ponto/services/cartorio.py:375`, `app/ponto/services/cartorio.py:379`).
- Portas principais: `criar_ausencia` cria com colaborador/tipo/datas/minutos/status; `editar_ausencia_registro` altera tipo/range/minutos; `mudar_estado` altera status e salva; `decidir` passa por `mudar_estado` e pode escrever `hora_ini/hora_fim`. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:341`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:508`, `app/ponto/services/ausencia.py:792`, `app/ponto/services/ausencia.py:819`, `app/ponto/services/ausencia.py:875`, `app/ponto/services/ausencia.py:881`, `app/ponto/services/ausencia.py:895`, `app/ponto/services/ausencia.py:908`, `app/ponto/services/ausencia.py:911`).
- Essas escritas por save/delete disparam cartório diretamente pelo receiver; dias vêm de `_dias_ausencia`, usando as datas **atuais** da instância, aberto até ontem e limitado a hoje, filtrando células existentes quando o range tem mais de três dias. Não há captura do range anterior nesse receiver/helper. (`app/ponto/signals.py:115`, `app/ponto/signals.py:123`, `app/ponto/signals.py:124`, `app/ponto/signals.py:127`, `app/ponto/signals.py:143`, `app/ponto/signals.py:147`, `app/ponto/signals.py:150`, `app/ponto/signals.py:152`).
- Views usuárias dessas portas incluem lançar/submeter, aprovar/rejeitar/cancelar individual e lote, editar registro e editar período parcial. (`app/ponto/views.py:2260`, `app/ponto/views.py:2280`, `app/ponto/views.py:2294`, `app/ponto/views.py:2309`, `app/ponto/views.py:2323`, `app/ponto/views.py:2341`, `app/ponto/views.py:2358`, `app/ponto/views.py:2405`, `app/ponto/views.py:2457`, `app/ponto/views.py:2526`, `app/ponto/views.py:2589`, `app/ponto/views.py:2652`, `app/ponto/views.py:2663`, `app/ponto/views.py:2670`, `app/ponto/views.py:2710`, `app/ponto/views.py:2741`, `app/ponto/views.py:2784`, `app/api/views.py:1738`, `app/api/views.py:1872`).
- Férias: `AgendamentoFerias.save()` chama `_sync_ausencia` → `espelhar_ferias`; este cria uma Ausencia aprovada por dia via `criar_ausencia` e apaga as que deixam de valer; o delete do agendamento também apaga ausências. São essas criações/deleções que acionam o cartório. (`app/ferias/models.py:384`, `app/ferias/models.py:434`, `app/ferias/models.py:437`, `app/ferias/models.py:439`, `app/ponto/services/ausencia.py:1144`, `app/ponto/services/ausencia.py:1172`, `app/ponto/services/ausencia.py:1186`, `app/ponto/services/ausencia.py:1195`, `app/ponto/services/ausencia.py:1246`, `app/ponto/services/ausencia.py:1254`).
- Cancelamento DP salva status cancelado; cancelamento admin primeiro rejeita ausências por update sem signal, depois salva o agendamento cancelado, cujo sync apaga as ausências e dispara post_delete. Correção de datas apaga o range antigo e salva/sincroniza o novo. (`app/ferias/services.py:330`, `app/ferias/services.py:337`, `app/ferias/services.py:341`, `app/ferias/services.py:356`, `app/ferias/services.py:362`, `app/ponto/services/ausencia.py:1257`, `app/ponto/services/ausencia.py:1267`, `app/ferias/services.py:366`, `app/ferias/services.py:385`, `app/ferias/services.py:387`).
- Nenhuma dessas portas enfileira rejulgamento em outbox; os saves/deletes chamam diretamente cartório e a eventual lavra emite o outbox posterior descrito acima. (`app/ponto/signals.py:93`, `app/ponto/signals.py:147`, `app/ponto/signals.py:152`, `app/ponto/portas/celula.py:638`).

**(vi) Feriados da praça**

- Não há argumento/chave de feriado no hash. Pode entrar indiretamente **se uma geração/regeneração atualizar os marcos**: `eh_dia_trabalho_calculado` aplica `feriado_vira_folga`, que consulta o feriado pelo município do posto; DNA só monta marcos quando `trabalha is True`. (`app/ponto/services/cartorio.py:314`, `app/ponto/services/cartorio.py:323`, `app/escala/models.py:1338`, `app/escala/models.py:1340`, `app/escala/models.py:1344`, `app/escala/models.py:1362`, `app/escala/models.py:1363`, `app/ponto/management/commands/gerar_celulas.py:79`, `app/ponto/management/commands/gerar_celulas.py:100`, `app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:260`).
- O alcance é nacional/UF/município; o lugar do colaborador vem de `colaborador.posto.municipio`. (`app/core/feriados.py:57`, `app/core/feriados.py:60`, `app/core/feriados.py:62`, `app/core/feriados.py:64`, `app/core/feriados.py:106`, `app/core/feriados.py:113`).
- Portas de calendário: `form_feriado` cria/edita; `_importar_do_payload`, chamado por `importar_feriados_nacionais`, faz get_or_create; `excluir_feriado` apaga. (`app/core/views_config.py:39`, `app/core/views_config.py:62`, `app/core/views_config.py:68`, `app/core/views_config.py:81`, `app/core/views_config.py:124`, `app/core/views_config.py:141`, `app/core/views_config.py:153`, `app/core/views_config.py:164`, `app/core/views_config.py:182`, `app/core/views_config.py:198`).
- Alteração do lugar também muda a aplicabilidade: portas `atualizar_posto`/`definir_municipio_posto`; a segunda usa update sem signal. (`app/colaboradores/services/estrutura.py:47`, `app/colaboradores/services/estrutura.py:50`, `app/colaboradores/services/estrutura.py:67`, `app/colaboradores/services/estrutura.py:70`).
- O signal de Feriado roda `reavaliar_dia(data)` em on_commit; não regenera células nem enfileira julgamento. Ele busca chamados `batida_ausente` vivos, consulta `fatos_do_dia` e reconcilia os espúrios. A reconciliação pode provocar rejulgamento **indireto** pela mudança de status do chamado. (`app/chamados/signals.py:265`, `app/chamados/signals.py:274`, `app/chamados/signals.py:276`, `app/chamados/services/feriado_reavaliar.py:14`, `app/chamados/services/feriado_reavaliar.py:33`, `app/chamados/services/feriado_reavaliar.py:43`, `app/chamados/services/feriado_reavaliar.py:47`, `app/chamados/services/feriado_reavaliar.py:59`, `app/chamados/signals.py:164`, `app/chamados/signals.py:180`).
- Não há post_delete de Feriado: o código declara isso expressamente, porque essa reavaliação só fecha chamados e não reabre retratados quando o feriado é removido. (`app/chamados/signals.py:261`, `app/chamados/signals.py:262`).
- O eventual efeito sobre chamados muda a parcela `pk:status_local` do hash; o feriado em si continua ausente. (`app/ponto/services/cartorio.py:326`).

**(ix) Isenção art. 62**

- Flag real: `Colaborador.isento_ponto`. A flag não entra diretamente no hash, nem no DNA produzido por `montar_dna`; contudo é entregue diretamente ao juiz como `isento_ponto=bool(c.isento_ponto)`. (`app/colaboradores/models.py:294`, `app/ponto/services/cartorio.py:323`, `app/ponto/services/cartorio.py:327`, `app/ponto/management/commands/gerar_celulas.py:95`, `app/ponto/services/cartorio.py:1113`).
- Não encontrei caminho que codifique a flag indiretamente no hash: o signal da isenção silencia chamados, conservando seu status, e o hash lê status, não `silenciado_ate`. (`app/chamados/signals.py:213`, `app/chamados/signals.py:222`, `app/chamados/services/silencio_isentos.py:34`, `app/chamados/services/silencio_isentos.py:43`, `app/chamados/services/silencio_isentos.py:46`, `app/ponto/services/cartorio.py:326`).
- Porta humana `passe_livre` chama `colaboradores.services.colaborador.gravar(colab,'cadastro',{'isento_ponto':novo_valor})`; `gravar` atribui e salva. (`app/colaboradores/views.py:145`, `app/colaboradores/views.py:173`, `app/colaboradores/views.py:176`, `app/colaboradores/services/colaborador.py:34`, `app/colaboradores/services/colaborador.py:47`, `app/colaboradores/services/colaborador.py:53`).
- Essa porta não chama cartório nem emite outbox. O recei

[truncado em 20000 de 20717 caracteres]

## 30/10 20:13 · sessao 82751e7ef508 · - · - · origem=Ronald (tmux)

Censo concluído, somente leitura. Não carreguei Django nem executei testes; usei leitura estática, `rg` e o varredor AST puro com `PYTHONDONTWRITEBYTECODE=1`. Resultado de execução vivo de `_meta` permanece [não verificado]. [nao verificado]

**Inventário: 24 campos concretos + 2 relações reversas = 26 entradas de `get_fields()` pela leitura estática.** A classe herda diretamente `models.Model` (`app/escala/models.py:1603`); são 23 declarações de campos (`app/escala/models.py:1649` até `:1773`) e `id` implícito BigAutoField, registrado na migration (`app/escala/migrations/0030_celuladia.py:20`; configuração `app/escala/apps.py:5`).

Nas referências abaixo, **P = `app/ponto/portas/celula.py`**. O save comum de lavratura/carimbo é `salvar_e_emitir`, P:634–642.

| Campo | Definição | Escritores de produção |
|---|---|---|
| `id` | migration 0030:20 | Automático nos creates de `regenerar_celulas_vinculo` P:247 e `gerar_celulas_periodo` P:593. |
| `colaborador` | models:1649 | Creates P:247 e P:594. |
| `data` | models:1653 | Creates P:247 e P:594. |
| `trabalha` | models:1654 | Creates P:247/P:595; regenerar vínculo P:231/P:260, saves P:233/P:264; gerar período P:612/P:615; reverter P:532/P:538. |
| `origem` | models:1659 | Somente `'gerada'` nos creates P:248/P:595. Nenhum escritor de produção de `'editada'` localizado. |
| `escala_geradora` | models:1660 | Creates P:248/P:595; regenerar P:260/P:264; gerar P:613/P:615; reverter P:535/P:538. `SET_NULL` declarado em models:1661. |
| `dna` | models:1665 | Creates P:248/P:596; regenerar P:232–233/P:260–265; gerar P:604–605/P:614–615; reverter P:530/P:538. |
| `gerada_em` | models:1670 | `auto_now_add` nos creates P:247/P:593. |
| `regenerada_em` | models:1686 | Regenerar P:262/P:264; reverter P:536/P:538. |
| `regeneracoes` | models:1691 | Default 0; regenerar P:263/P:264; reverter P:537/P:538. |
| `dna_anterior` | models:1694 | Regenerar P:261/P:264; reverter limpa P:531/P:538. |
| `editada_em` | models:1700 | Nenhum escritor de produção localizado. |
| `editada_por` | models:1701 | Nenhum escritor de produção localizado; `SET_NULL` declarado em models:1702. |
| `julgada_em` | models:1713 | `carimbar`, P:721/P:726. |
| `impressao` | models:1714 | `carimbar`, P:722/P:726. |
| `veredito` | models:1738 | `lavrar_veredito`, P:692/P:695. |
| `veredito_via` | models:1739 | `lavrar_veredito`, P:693/P:695. |
| `veredito_em` | models:1740 | `lavrar_veredito`, P:694/P:695. |
| `protestos` | models:1741 | `lavrar_veredito`, P:691/P:695. |
| `ata` | models:1746 | `lavrar_veredito`, P:674/P:690/P:697; `carimbar_precedencia`, P:739–743. |
| `ausencia` | models:1764 | `carimbar`, P:723/P:726; `carimbar_insumos_em_lote`, P:759/P:764. `SET_NULL` models:1765. |
| `tem_entrada` | models:1769 | `carimbar`, P:724/P:726; lote P:760/P:764. |
| `tem_saida` | models:1770 | `carimbar`, P:724/P:726; lote P:760/P:764. |
| `insumos_em` | models:1773 | `carimbar`, P:725/P:727; lote P:761/P:764. |
| `chamados` reverso | `app/chamados/models.py:44–45` | Não é coluna da célula: nasce pela FK no create de chamado, `models.py:876`/`:905`; associação posterior por `services/vinculos.py:92–99`. |
| `perguntas` reverso | `app/chamados/models.py:1943–1944` | Não é coluna da célula: `PerguntaDisputa.abrir` reaponta FK em `models.py:2167–2179` ou cria em `:2181–2182`. |

`regenerar_celulas_dia` apenas delega ao regenerador de vínculo com `[dia,dia]`, sem extensão (`P:474–477`). O wrapper de modelo `CelulaDia.lavrar_veredito` delega para a porta (`app/escala/models.py:1775–1783`).

**Não existe relação reversa `eventos_consumo` localizada.** `EventoCelula.celula_id` é `IntegerField`, explicitamente sem FK (`app/escala/models.py:1806–1809`).

**Selos encontrados:**

- Escritor único por módulo, allowlist vazia: `app/ponto/tests/test_chokepoint_celula_gate.py:20–22`, teste da árvore `:49–53`, entidades `:58–60`, operações declaradas e cobertura AST `:62–84`, contraprovas `:86–96`.
- Cadastro da família/porta: `app/core/portas.py:44`, `:111`.
- Varredor AST: `app/core/censo_escritas.py:6–45`; as classes de inferência estão documentadas ali, inclusive balde não atribuível.
- Contraprovas do censo envolvendo CelulaDia: `app/core/tests/test_censo_escritas.py:15–16`, `:72–90`; gerente reverso `:143–157`; produção exclui testes e inclui management `:230–261`.
- Idempotência das operações: `app/ponto/tests/test_porta_celula.py:36–60`, lavrar `:63–76`, carimbar `:79–92`, precedência/ata `:94–139`, lote `:142–150`.
- Veredito verifica conjunto dos **quatro campos retornados pela lavratura**, não conjunto de fields do modelo: `app/escala/tests/test_b5_veredito.py:37–42`.
- **Nenhum teste do conjunto completo de fields de CelulaDia localizado** ao buscar `CelulaDia._meta`, `_meta`, `get_fields`, `escritor unico`, `escritor_unico`, `escritores` em `app/*/tests`. Os selos acima são de módulo/operacão ou comportamento, não um mapa completo campo→escritores. [nao verificado]

**(iii) DNA:**

- Hash só usa `dna_versao` e `str(marcos)`, não o JSON inteiro (`app/ponto/services/cartorio.py:327`). Logo alterar outra chave isoladamente não muda diretamente hash.
- Portas que realmente gravam: gerar período, regenerar vínculo/dia e reverter, nos sítios da tabela.
- Regenerar vínculo, com `recalculo=True`, julga diretamente os dias tocados não futuros e sem veredito humano, com `forcar=True` (`P:360–379`). O wrapper por dia usa esse default (`P:474–477`).
- Gerar período termina sem chamada ao cartório/outbox (`P:590–617`). Reverter termina após save/trilha, sem julgamento/outbox (`P:529–552`).
- Não encontrei receiver de `CelulaDia` em `app/`; comentário declara ausência intencional do signal (`P:332–334`).
- Regeneração não insere diretamente `EventoCelula`; uma lavra posterior pode inserir se mudar `veredito` ou `ata` (`P:622–642`).

**(viii) Decisões humanas:**

1. **Justificativa**: `status`, aprovação, tipo etc. não entram diretamente na função hash; não há parâmetro/chave de justificativa (`cartorio.py:314–380`). Pode mudar hash por batida criada, por status dos chamados fechados e por `fio`; a justificativa sem esses efeitos pode manter impressão idêntica, apesar de signal chamar julgamento (`cartorio.py:324–327`, `:367–368`; sinais abaixo).
   - Individual: `aprovar_justificativa_dp` (`app/ponto/services/justificativa.py:117`, grava `:190–193`), `rejeitar_justificativa_dp` (`:247`, `:268–272`), `materializar_saida_dp` (`:316`, `:368–371`).
   - Lote: `aprovar_lote_justificativas` (`app/ponto/views.py:1764`, grava `:1797–1800`), rejeitar lote (`:1860`, grava `:1876–1880`).
   - Admin genérico editável: `JustificativaAdmin(admin.ModelAdmin)` (`app/ponto/admin.py:18–22`); dívida reconhecida pelo selo em `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:12–15`.
   - Todo save aciona julgamento **síncrono**, não fila de rejulgamento: `app/ponto/signals.py:161–165` → `:85–93`.
   - Aprovação pode criar batida (`services/justificativa.py:172–179`) e fecha origem (`:202`); lote cria batida (`views.py:1816–1822`) e fecha origem (`:1838`).

2. **Edição de célula `origem='editada'`**: **nenhuma porta de produção localizada**, embora modelo reconheça a origem (`app/escala/models.py:1658–1659`) e gerador a preserve (`P:598–599`). `editada_em/editada_por` sem escritores, como acima. Não afirmar que uma porta humana de edição existe só por comentários do modelo (`models.py:1608–1611`).

3. **Veredito humano diretamente no dia**: view `app/ponto/views.py:3302–3304` → serviço `veredito_celula` (`app/ponto/services/veredito_celula.py:191`).
   - `trabalhou` pode flipar/gravar batidas (`:291–305`); `faltou/justificado` cria Ausencia (`:310–317`); fecha fio (`:324–328`); escreve veredito admin (`:329–330`).
   - `veredito_via`, veredito e autor não entram diretamente hash (`cartorio.py:323–380`). Seus fatos materiais entram pelas batidas/cobertura/chamados/fio.
   - A lavra admin chama `salvar_e_emitir` (`P:682–697`) e **gera EventoCelula se houver mudança**, pelo mesmo atomic (`P:634–642`). Esse outbox reconcilia chamados, não enfileira rejulgamento do próprio cartório (`app/escala/models.py:1803–1807`).

4. **Decisão em chamado**: `ChamadoColaborador.fechar` → `transicionar` → `_gravar_estado` (`app/chamados/models.py:260–264`), save em `:231`.
   - Entra no hash por `pk:status_local` (`cartorio.py:326`).
   - Criar/mudar status aciona cartório síncrono: pre/post-save `app/chamados/signals.py:155–180`, sem outbox de rejulgar.

5. **Decisão em pergunta**:
   - `validar_pergunta` (`app/chamados/services/validacao.py:336`) carimba (`:389`, escritor `:127–133`) e materializa (`:415–423`).
   - `recusar_pergunta` (`:487`) salva recusa e chama reabertura (`:538–540`).
   - Fecho admin explícito `PerguntaDisputa.fechar_por_admin` (`app/chamados/models.py:2213–2230`).
   - Pergunta muda hash **indiretamente via contagem de chamados com fio vivo** (`cartorio.py:1074`, `:1087`).
   - Receiver direto da pergunta só fecha beco da disputa, `on_commit`, **sem chamada a julgar_celula e sem inserir outbox** (`app/chamados/signals.py:191–202`).
   - Validação pode disparar julgamento pelos fatos materializados (Batida/Ausencia/Justificativa); o `recalcular_por_evento` da validação (`validacao.py:399–401`) é do fechamento financeiro, não fila do cartório.

## 30/10 20:12 · sessao b72efd86392b · - · - · origem=Ronald (tmux)

CCT / item 2(vii)

- **Não entra no hash diretamente nem indiretamente pelo DNA**: `impressao_insumos` só concatena batidas, status da cobertura, chamados, `dna_versao`, `marcos`, veto, teto, versão da regra, encerramento, fio e parcialidade; nenhum campo da CCT aparece (`app/ponto/services/cartorio.py:314-380`). O produtor do DNA não lê CCT: usa `EscalaColaborador`, `TipoEscala`, marcos, jornada, intervalos e extras declaradas (`app/ponto/management/commands/gerar_celulas.py:73-123`).
- **O argumento `regra` não é a régua CCT**: vem de `versao_para`/`REGRA_VERSOES`, declaração de versão do código do cartório (`app/ponto/services/cartorio.py:100`, `app/ponto/services/cartorio.py:168-187`, `app/ponto/services/cartorio.py:888-891`). Alterar `Empresa.regime_trabalhista`, Sindicato ou AplicacaoConvencao não altera essa versão (`app/ponto/services/cartorio.py:168-187`).
- A régua efetiva é decidida por `regua_para`: vínculo do dia → praça e ciclo (`app/core/regua_cct.py:328-339`); `Empresa.regime_trabalhista` decide CLT/CCT (`app/core/regua_cct.py:352-366`); `_aplicacao_vigente` busca aplicações ativas da empresa, praça ou empresa inteira, confere vigência do Sindicato, prioriza praça e recusa empate (`app/core/regua_cct.py:253-275`); os valores são extraídos do Sindicato (`app/core/regua_cct.py:374-380`).
- `get_motor_cct` lê o switch global `ParametroSistema.regua_cct_ativa` e injeta a régua no motor de cálculo; trata-se do caminho do motor/folha (`app/core/regua_cct.py:428-470`). A régua expõe sete campos declarados sem efeito no cálculo e quatro que chegam ao motor (`app/core/regua_cct.py:72-89`).

**Portas que mudam CCT/regime e seus disparos**

| Estado | Porta de produção | Rejulgamento / outbox |
|---|---|---|
| Campos/vigência de Sindicato | `form_sindicato`: coleta régua (`app/core/views_config.py:258`, `app/core/views_config.py:268-297`), atualiza e salva (`app/core/views_config.py:303-321`) ou cria (`app/core/views_config.py:323-336`). | Não chama juiz nem emite evento: termina em trilha, atualização de `VinculoSindicatoPraca` e resposta (`app/core/views_config.py:320-343`). |
| AplicacaoConvencao | `form_aplicacao` chama `cadastrar` ou `desativar` (`app/core/views_config.py:387`, `app/core/views_config.py:402-417`). `semear_aplicacao_convencao.handle` chama as mesmas portas (`app/core/management/commands/semear_aplicacao_convencao.py:105-111`). Escritor: criação/reativação/desativação em `cadastrar` (`app/core/services/aplicacao_convencao.py:101-134`) e desativação em `desativar` (`app/core/services/aplicacao_convencao.py:138-157`). | Não chama juiz nem outbox: salva e registra auditoria, depois retorna (`app/core/services/aplicacao_convencao.py:120-134`, `app/core/services/aplicacao_convencao.py:149-157`). |
| Empresa.regime_trabalhista | `form_empresa` valida o valor recebido (`app/colaboradores/views.py:485`, `app/colaboradores/views.py:512-515`) e passa às portas `atualizar_empresa` ou `criar_empresa` (`app/colaboradores/views.py:519-540`); escritores genéricos em `app/colaboradores/services/estrutura.py:12-20`. | Não chama juiz nem outbox: as portas só fazem create/setattr/save (`app/colaboradores/services/estrutura.py:12-21`). |

- O admin não é porta alternativa para esses dois estados: regime é readonly (`app/colaboradores/admin.py:19`); AplicacaoConvencao usa `SoLeitura` (`app/core/admin.py:99-102`), que proíbe adicionar, mudar e apagar (`app/core/admin.py:59-66`).
- Não encontrei receivers de `Empresa`, `Sindicato` ou `AplicacaoConvencao` buscando `sender` em todos os `.py` de `app/`; os signals inspecionados não contêm esses ganchos (`app/core/signals.py:1-48`, `app/colaboradores/signals.py:1-119`, `app/ponto/signals.py:85-165`, `app/chamados/signals.py:155-202`).
- Não encontrei escritor de produção do switch `regua_cct_ativa` pesquisando o nome e `ParametroSistema` em `app/core/views_config.py`, `app/core/services/` e `app/core/management/commands/`; a tela de parâmetros é somente leitura e recusa POST (`app/core/views_config.py:480-492`). [não verificado fora desse escopo]

**Mecanismo de outbox — distinção essencial**

- O outbox real é `escala.models.EventoCelula`, com `celula_id`, `colaborador_id`, data, criação, processamento, tentativas e erro (`app/escala/models.py:1800-1815`).
- Ele **não é uma fila de rejulgamento do cartório**: registra a lavra da célula e acorda o consumidor de chamados (`app/escala/models.py:1801-1807`). O consumidor chama `retratar_chamados_absolvidos_por_celula` e `encerrar_orfas_pela_lampada`, não o juiz do cartório (`app/chamados/reconciliador.py:1257-1268`, `app/chamados/reconciliador.py:1295-1306`).
- A emissão é centralizada em `emitir_evento_celula` (`app/ponto/portas/celula.py:626-631`). `salvar_e_emitir` grava célula e evento no mesmo atomic, somente se os campos lavrados incluem `veredito` ou `ata` (`app/ponto/portas/celula.py:622-642`). A mera gravação de DNA ou dos insumos do carimbo não aciona esse critério (`app/ponto/portas/celula.py:623`, `app/ponto/portas/celula.py:708-728`).
- O nascimento de chamado de turno ligado à célula também emite esse evento (`app/chamados/models.py:735-744`); há semeadura pela mesma porta (`app/chamados/reconciliador.py:1223-1253`).
- O consumidor roda a cada minuto (`app/config/crons.py:375`), seleciona pendentes com menos de cinco tentativas (`app/chamados/reconciliador.py:1154`, `app/chamados/reconciliador.py:1191-1200`), mantém pendências por falha/adiamento ou marca processamento (`app/chamados/reconciliador.py:1311-1320`).
- O rejulgamento reativo dos insumos é **chamada síncrona direta**: `_julgar_dias` chama `cartorio.julgar_celula`, com guarda antirrecursão (`app/ponto/signals.py:85-93`); os receivers de Batida, Ausencia e Justificativa chamam esse helper (`app/ponto/signals.py:131-165`). Mudança/criação de chamado chama diretamente o mesmo juiz (`app/chamados/signals.py:155-180`).
- A “fila” do comando `processar_cartorio` é um queryset de colaboradores com células passadas, não tabela de pendências (`app/ponto/management/commands/processar_cartorio.py:45-65`); o cron o chama com `--apply`, sem `--forcar` (`app/config/crons.py:597`). O pulo usa igualdade da impressão (`app/ponto/services/cartorio.py:1092-1094`).
- `out['rejulgar']` também não é outbox: é lista em memória consumida na segunda passada; sobras são descartadas e contadas (`app/ponto/services/cartorio.py:745-753`).
- Não encontrei modelo próprio `*Outbox*`, `*Pendencia*` ou `*Fila*` de rejulgamento do cartório no censo de classes/modelos e ocorrências `outbox|Outbox|enfileirar|julgar_celula|julgar_colab` em `app/**/*.py`, excluindo migrations e testes; o mecanismo concreto encontrado é o acima (`app/ponto/management/commands/processar_cartorio.py:49-65`, `app/ponto/signals.py:85-93`, `app/escala/models.py:1800-1815`, `app/chamados/reconciliador.py:1295-1306`).

## 30/10 19:54 · sessao 138b78f0be18 · O261-E-CELULA-20 · censo · origem=Code

**O bloqueio nasce da ordem das operações:** F1 exige que a pergunta deixe de ser viva antes de fechar o chamado; nesse caminho, a célula só deixa de acusar depois do fechamento e do rejulgamento. O C6 continua vendo a célula acusadora durante a nova guarda (`app/chamados/models.py:601`; `app/ponto/services/cartorio.py:1163`; `app/ponto/services/cartorio.py:1166`; `app/chamados/juizes.py:1056`).

Uso `B/` para os arquivos da base no diretório `e54eae6ed8c46bc652434308bf918485dd3ac23e`; caminhos sem esse prefixo são da cópia. Nos anexos, numero as linhas desde `Found 29 test(s).`.

**(1) Escritor e leitores de `cobrado`**

O cartório escolhe `_vd='cobrado'` quando, simultaneamente:

- `cods` está vazio;
- existe chamado vivo naquele dia;
- algum desses chamados tem pergunta viva entre `perguntas_que_resolvem`. O predicado do fio termina em `pergunta_viva` (`app/ponto/services/cartorio.py:1069`; `app/ponto/services/cartorio.py:1074`; `app/ponto/services/cartorio.py:1122`; `app/ponto/services/cartorio.py:1141`; `app/chamados/juizes.py:1111`).

A escolha entra em `cel.lavrar_veredito`; a atribuição efetiva é `cel.veredito = v` na porta da célula. O cartório persiste pelo `carimbar`. Uma decisão anterior com `veredito_via='admin'` impede essa substituição pelo cartório (`app/ponto/services/cartorio.py:1409`; `app/ponto/portas/celula.py:672`; `app/ponto/portas/celula.py:692`; `app/ponto/services/cartorio.py:1424`; `app/ponto/portas/celula.py:728`).

`VEREDITOS_ACUSAM` contém `furo`, `discordante`, `indefinida`, `cobranca_indevida`, **`cobrado`**, `nunca_bateu` e `fato_em_ausencia`. O C6 retorna `True` ao encontrar esse pertencimento. A saída anterior por marco aceso exige `materializada_em` preenchido (`app/escala/models.py:1735`; `app/chamados/juizes.py:1050`; `app/chamados/juizes.py:1056`).

Censo das leituras diretas do literal ou da constante, fora de testes e migrations: [nao verificado]

| Uso | Arquivo:linha |
|---|---|
| C6 e consulta de perguntas vivas | `app/chamados/juizes.py:1002`, `app/chamados/juizes.py:1056`, `app/chamados/models.py:2523`, `app/chamados/models.py:2524` |
| Absolvição de perguntas e chamados | `app/chamados/reconciliador.py:598`, `app/chamados/reconciliador.py:978` |
| Emissão, silêncio, resposta tardia e LIMBO | `app/chamados/services/fabrica_da_celula.py:86`, `app/chamados/services/disputa_emissao.py:705`, `app/chamados/services/silencio_ausencia.py:41`, `app/chamados/services/resposta_tardia.py:45`, `app/chamados/services/limbo_folgas.py:194` |
| Folha e apresentação | `app/folha/export.py:1215`, `app/relatorios/dia_da_frota.py:55`, `app/relatorios/furos.py:25`, `app/colaboradores/services/calendario.py:70` |
| Vigias | `app/chamados/services/furo_sem_canal.py:35`, `app/chamados/services/vigia_fio.py:182`, `app/folha/services/vigia_pausa.py:155`, `app/ponto/services/vigia_ausencia_veredito.py:41` |
| Comandos | `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`, `app/chamados/management/commands/disparar_perguntas_competencia.py:86`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:133`, `app/chamados/management/commands/auditar_invariantes_chamados.py:197`, `:342`, `:1387`, `:1601`, `:1643` |

**(2) Sequência da base**

1. `registrar_batida` cria a batida; o receiver do cartório chama `_julgar_dias`, que chama `julgar_celula` (`B/app/ponto/registro_batida.py:157` → `B/app/ponto/signals.py:132` → `B/app/ponto/signals.py:135` → `B/app/ponto/signals.py:93`).

2. `julgar_celula` chama `julgar_colab`; este executa `_julgar_colab_corpo`. O corpo monta a ata atual, disponibiliza-a no contexto do julgamento e chama `x.reconciliar()` quando os marcos cobrados acenderam (`B/app/ponto/services/cartorio.py:1560` → `B/app/ponto/services/cartorio.py:744` → `B/app/ponto/services/cartorio.py:1148` → `B/app/ponto/services/cartorio.py:1152` → `B/app/ponto/services/cartorio.py:1163`).

3. `reconciliar` consulta motor e premissa. Com a premissa regularizada, chama `_gravar_estado`, que atribui o novo status e salva (`B/app/chamados/models.py:440` → `B/app/chamados/models.py:467` → `B/app/chamados/models.py:470` → `B/app/chamados/models.py:495` → `B/app/chamados/models.py:444` → `B/app/chamados/models.py:192` → `B/app/chamados/models.py:231`).

4. **É após esse save que existe o intervalo “chamado terminal, pergunta ainda viva”.** A célula ainda pode apresentar o veredito acusador anterior, e a pergunta sem materialização não passa pela exceção do marco aceso. O C6 retorna viva antes de chegar ao fallback que examina o destino terminal. O receiver do chamado evita rejulgamento recursivo durante o cartório (`B/app/chamados/models.py:231`; `B/app/chamados/juizes.py:1050`; `B/app/chamados/juizes.py:1056`; `B/app/chamados/juizes.py:1083`; `B/app/chamados/signals.py:174`).

5. O fechamento enfileira a célula para rejulgamento. A primeira lavra ainda pode usar `_vd='cobrado'`, calculado antes do fechamento; a segunda passada relê os vivos. Sem protestos e sem aquela cobrança viva, deriva `concorde` (`B/app/ponto/services/cartorio.py:1142` → `B/app/ponto/services/cartorio.py:1166` → `B/app/ponto/services/cartorio.py:1409` → `B/app/ponto/services/cartorio.py:746` → `B/app/ponto/services/cartorio.py:1069`; `B/app/escala/models.py:1596`).

6. O receiver posterior da batida chama `reconciliar_chamados_colab`. Seu primeiro consumidor de absolvição seleciona células não acusadoras, confirma C6 falso e grava `materializada_em`, autor e `via_resolucao='celula_absolveu'` (`B/app/ponto/signals.py:207` → `B/app/ponto/signals.py:215` → `B/app/chamados/reconciliador.py:1350` → `B/app/chamados/reconciliador.py:598` → `B/app/chamados/reconciliador.py:613` → `B/app/chamados/reconciliador.py:617` → `B/app/chamados/reconciliador.py:619`).

O resultado fornecido confirma um chamado fechado e a pergunta materializada por `celula_absolveu`, célula `concorde`, C6 falso (`anexo debug_base.log:4`; `anexo debug_base.log:5`).

**(3) Dependências que retêm a cópia**

- **Fechamento → pergunta:** `_decisao_do_fechamento` consulta primeiro `_plano_pergunta_viva`. Havendo viva sem reata válida, retorna `None` antes do gate da premissa (`app/chamados/models.py:601`; `app/chamados/models.py:604`; `app/chamados/models.py:605`).
- **Pergunta → célula:** `_vivas_ancoradas` simula somente o status novo do chamado. Ela não simula a futura lavra da célula. O C6 continua lendo o veredito acusador atual (`app/chamados/models.py:304`; `app/chamados/models.py:310`; `app/chamados/models.py:311`; `app/chamados/juizes.py:1056`).
- **Marco aceso → materialização prévia:** a luz acesa só encerra a pergunta nesse degrau se ela já tiver `materializada_em`. A pergunta da sonda não tem esse carimbo (`app/chamados/juizes.py:1050`; `anexo debug_cura.log:7`).
- **Chamado vivo + pergunta viva → `cobrado`:** recusado o fechamento, ambos permanecem no cálculo do fio. Sem protestos, o cartório escolhe `cobrado`, que também acusa (`app/ponto/services/cartorio.py:1074`; `app/ponto/services/cartorio.py:1122`; `app/ponto/services/cartorio.py:1142`; `app/escala/models.py:1736`).
- **Célula acusadora → ausência de `celula_absolveu`:** o consumidor que grava esse carimbo exclui `VEREDITOS_ACUSAM` (`app/chamados/reconciliador.py:598`).

O pós-estado simulado do destino não rompe essa dependência: o ramo da célula acusadora vem antes do fallback de materialização que considera destino encerrado (`app/chamados/juizes.py:1056`; `app/chamados/juizes.py:1083`; `app/chamados/juizes.py:700`).

Na sonda, a tentativa vê pergunta #1 `saida_sem_entrada`, célula `furo`, e recusa; depois ficam chamado aberto, célula `cobrado` e pergunta viva (`anexo debug_cura.log:4`; `anexo debug_cura.log:5`; `anexo debug_cura.log:7`).

**(4) Os três testes** [nao verificado]

**PORTA-2ATO / MORDE.** O chamado identificado pelo teste é `ch`, módulo `batida_ausente`, subcategoria `marco_furado`, dia `self.seg`; a exigência é sair de `VIVOS` depois da correção (`app/colaboradores/tests/test_contract_porta_2ato.py:150`; `app/colaboradores/tests/test_contract_porta_2ato.py:158`).

A regeneração altera `trabalha` e DNA sem limpar o veredito. O cartório tenta reconciliar cobranças do dia sem trabalho antes de lavrar o novo veredito. Assim, **uma pergunta ancorada que ainda leia célula acusadora pode barrar essa tentativa**, embora o previsto já tenha virado folga (`app/ponto/portas/celula.py:260`; `app/ponto/portas/celula.py:264`; `app/ponto/services/cartorio.py:1188`; `app/ponto/services/cartorio.py:1196`; `app/ponto/services/cartorio.py:1409`; `app/chamados/juizes.py:1056`; `app/chamados/models.py:601`).

A fixture não cria explicitamente a pergunta retentora; existe geração automática pelo save do chamado e pela fábrica. **Qual pergunta efetivamente o segura — PK, motivo e ramo C6 observado — é [nao verificado] nos anexos fornecidos** (`app/colaboradores/tests/test_contract_porta_2ato.py:147`; `app/chamados/signals.py:180`; `app/ponto/services/cartorio.py:770`; `app/chamados/services/fabrica_da_celula.py:124`).

**T34 / furo sob LIMBO.** Quem deixa de fechar é `self.furo`, `batida_ausente`. A pergunta explícita é `p`, motivo `orfao_14h`, tipo `hora`, destino `self.furo`. O próprio teste exige furo resolvido **e essa pergunta viva, sem materialização** (`app/chamados/tests/test_t34_furo_sob_limbo.py:23`; `app/chamados/tests/test_t34_furo_sob_limbo.py:42`; `app/chamados/tests/test_t34_furo_sob_limbo.py:47`; `app/chamados/tests/test_t34_furo_sob_limbo.py:52`; `app/chamados/tests/test_t34_furo_sob_limbo.py:53`).

`agrupar_furo` grava `agrupado_em` antes de reconciliar. A premissa aceita agrupamento sob pai vivo, mas F1 pode recusar pela pergunta antes de consultar essa exceção. O pai LIMBO não integra os módulos de cobrança pesquisados para reata (`app/chamados/services/limbo_folgas.py:219`; `app/chamados/services/limbo_folgas.py:221`; `app/chamados/models.py:644`; `app/chamados/models.py:601`; `app/chamados/models.py:331`; `app/chamados/catalogo/modulos.py:235`).

Sem absolvição anterior, pergunta sem materialização permanece viva; célula acusadora também retorna viva. **Qual desses ramos foi percorrido nessa execução é [nao verificado] nos anexos** (`app/chamados/juizes.py:1056`; `app/chamados/juizes.py:1076`).

**O199 / quatro irmãs.** O chamado é `ch`, módulo `turno_aberto`. As quatro perguntas são `saida_sem_entrada`, respondidas `FOLGA_CONTESTA`, destinadas a `ch`, inicialmente sem validação/materialização (`app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:106`; `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:121`; `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:175`).

O primeiro aceite valida somente a pergunta atual antes de declarar folga. O fechamento do fio carimba as irmãs por `fechar_por_admin`, mas essa porta não preenche `validada_em`. As irmãs respondidas e ainda não validadas continuam vivas: o ramo da resposta precede a morte por identidade `admin_decidiu`; `FOLGA_CONTESTA` não oferece hora comparável com a luz (`app/chamados/services/validacao.py:283`; `app/ponto/services/veredito_celula.py:174`; `app/chamados/models.py:2399`; `app/chamados/juizes.py:1020`; `app/chamados/juizes.py:1032`; `app/chamados/juizes.py:1044`; `app/chamados/juizes.py:995`).

São essas irmãs que retêm a tentativa de `reconciliar` do fechamento do fio. Os aceites seguintes encontram folga pronta, validam a pergunta e retornam sem nova tentativa de reconciliar o chamado; por isso a recusa anterior pode permanecer após os quatro aceites (`app/ponto/services/veredito_celula.py:183`; `app/chamados/models.py:601`; `app/chamados/services/validacao.py:261`; `app/chamados/services/validacao.py:270`).

**(5) O significado de `encerradas=2`**

`encerradas` conta **perguntas carimbadas por `encerrar_fantasma()`**, uma por retorno verdadeiro da porta. O incremento ocorre antes das guardas de fechamento da disputa; não conta chamados nem disputas encerrados (`app/chamados/reconciliador.py:1095`; `app/chamados/reconciliador.py:1104`; `app/chamados/reconciliador.py:1106`; `app/chamados/models.py:2438`).

O universo abrange o colaborador inteiro: perguntas sem materialização/validação, com dia, em disputa aberta e com os dois motivos previstos. **O SQL não exige `chamado_destino=NULL`**, apesar do nome `_orfas_abertas` (`app/chamados/reconciliador.py:991`; `app/chamados/reconciliador.py:999`; `app/chamados/reconciliador.py:1082`).

Para contar, a pergunta precisa ter resposta vazia, luz do próprio marco acesa, C6 vivo e dia passado encerrado. Portanto, `self.respondida`, com resposta `06:55`, não é a segunda contada (`app/chamados/reconciliador.py:1033`; `app/chamados/reconciliador.py:1035`; `app/chamados/reconciliador.py:1037`; `app/chamados/reconciliador.py:1041`; `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:93`).

A mudança relevante é a retenção anterior pelo novo gate: a base consultava motor e premissa; a cópia recusa primeiro por pergunta viva sem reata. A fixture cria batidas antes das perguntas explícitas, e essas batidas podem gerar perguntas automaticamente pelo cartório/fábrica (`B/app/chamados/models.py:467`; `B/app/chamados/models.py:470`; `app/chamados/models.py:601`; `app/chamados/models.py:604`; `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:70`; `app/ponto/signals.py:135`; `app/ponto/services/cartorio.py:770`).

A explicação estática para o aumento é a conservação de uma candidata automática que antes podia ser encerrada/carimbada durante o setup. **A identidade exata da segunda pergunta, e a confirmação dessa trajetória individual, são [nao verificado] pelo dado fornecido.**

## 30/10 19:53 · sessao ade1699d4ba3 · - · - · origem=Ronald (tmux)

Q4 — referências da cópia:

- **PORTA-2ATO / MORDE:** o chamado é `ch`, `modulo_origem='batida_ausente'`, `subcategoria='marco_furado'`, com `data_turno=self.seg`; o teste exige que saia de `VIVOS` após a correção (`app/colaboradores/tests/test_contract_porta_2ato.py:147-160`). A fixture não cria pergunta explicitamente; seu nascimento pode ocorrer no `post_save` do chamado → `julgar_celula` → fábrica (`app/chamados/signals.py:164-180`; `app/ponto/services/cartorio.py:768-770`; `app/chamados/services/fabrica_da_celula.py:112-124`). O motivo/PK exato que o segura não consta nos anexos: **[nao verificado]**.
  
  A dependência estática: a regeneração troca `trabalha`/DNA sem limpar o veredito (`app/ponto/portas/celula.py:259-265`); o cartório tenta reconciliar a cobrança em dia sem trabalho antes de lavrar o novo veredito (`app/ponto/services/cartorio.py:1188-1199`, `1406-1425`). O C6 lê o veredito persistido da célula e retorna viva para um veredito acusador (`app/chamados/juizes.py:1053-1057`). Portanto, **se a pergunta ancorada ainda vê `furo`/`cobrado`, ela bloqueia essa tentativa**, mesmo após o DNA virar folga (`app/chamados/models.py:599-608`). Marcos início/E e fim/S correspondem a `saida_sem_entrada` e `orfao_14h`, respectivamente (`app/chamados/services/declaracao_texto.py:43-44`; `app/chamados/services/fabrica_da_celula.py:58-64`).

- **T3.4 / furo sob LIMBO:** quem deixa de fechar é `self.furo`, módulo `batida_ausente`, aberto, dia `self.d`; o pai é `self.pai`, `sem_previsao_escala/folga_nao_definida` (`app/chamados/tests/test_t34_furo_sob_limbo.py:21-25`). A pergunta explícita é `p`, motivo `orfao_14h`, destino `self.furo`, tipo `hora`, aberta sem resposta (`app/chamados/tests/test_t34_furo_sob_limbo.py:41-42`). O próprio contrato exige **furo resolvido com essa pergunta ainda viva e sem materialização** (`app/chamados/tests/test_t34_furo_sob_limbo.py:47-53`).
  
  `agrupar_furo` grava `agrupado_em` e depois chama `reconciliar` (`app/chamados/services/limbo_folgas.py:214-222`). A premissa antiga permite fechar agrupado sob pai vivo (`app/chamados/models.py:644`), mas F1 consulta as perguntas antes e pode recusar antes de chegar à premissa (`app/chamados/models.py:599-608`). A pergunta sem materialização continua viva na ausência de absolvição anterior; célula acusadora também a mantém viva (`app/chamados/juizes.py:1054-1057`, `1076-1077`). O pai LIMBO não serve de sobrevivente C8 nessa composição: F1 busca apenas `CALADOS_POR_ISENCAO`, que não inclui `sem_previsao_escala` (`app/chamados/models.py:327-334`; `app/chamados/catalogo/modulos.py:235-241`; `app/chamados/juizes.py:1620-1626`). O ramo C6 efetivamente percorrido nessa execução não está nos anexos: **[nao verificado]**.

- **O199 / quatro irmãs:** quem fica retido é `ch`, `turno_aberto/saida_sem_entrada`, `aberto`, dia informado no contexto (`app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:106-118`). São quatro perguntas de `hora`, motivo `saida_sem_entrada`, todas respondidas `FOLGA_CONTESTA`, apontando para `ch`, inicialmente sem validação/materialização (`app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:120-126`, `175-178`).
  
  O primeiro aceite valida somente a pergunta atual antes de declarar folga (`app/chamados/services/validacao.py:282-285`). O fechamento do fio carimba as irmãs por `fechar_por_admin`, que escreve `via_resolucao`/`materializada_em`, sem validar (`app/ponto/services/veredito_celula.py:174-178`; `app/chamados/models.py:2399-2402`). **As irmãs respondidas e ainda não validadas seguem vivas:** no C6, resposta não vazia vem antes da morte por identidade `admin_decidiu` ou por célula absolvida (`app/chamados/juizes.py:1017-1045`, `1053-1065`); `FOLGA_CONTESTA` não fornece HH:MM comparável, logo não satisfaz a concordância com a luz (`app/chamados/juizes.py:983-997`). Elas seguram o `reconciliar` executado pelo fechamento do fio (`app/ponto/services/veredito_celula.py:182-183`; `app/chamados/models.py:599-608`).
  
  Os próximos aceites encontram a folga pronta, validam/fecham cada pergunta e retornam sem tentar reconciliar novamente o chamado (`app/chamados/services/validacao.py:261-272`). Assim, todas podem terminar validadas enquanto `ch` conserva a recusa anterior; o teste exige `ch.status_local == 'resolvido'` ao final (`app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:177-184`). O PK da primeira irmã validada e das retentoras em runtime não consta nos anexos: **[nao verificado]**.

## 30/10 19:53 · sessao 956521c1388b · - · - · origem=Ronald (tmux)

Q1/Q2, somente leitura:

- O cartório escolhe `cobrado` quando `cods` está vazio, há chamados em `VIVOS` e pelo menos um desses chamados tem fio vivo; o fio usa `pergunta_viva`. (`app/ponto/services/cartorio.py:1069`, `:1074`, `:1122`, `:1141`, `:1142`; `app/chamados/juizes.py:1111`).
- O valor chega à porta por `cel.lavrar_veredito(... save=False)`; a porta respeita decisão de admin, atribui `cel.veredito = v`, e `carimbar` persiste junto dos demais campos. (`app/ponto/services/cartorio.py:1409`, `:1424`; `app/ponto/portas/celula.py:672`, `:692`, `:728`).
- `cobrado` pertence a `VEREDITOS_ACUSAM`; C6 retorna vivo por esse pertencimento. A exceção anterior de marco aceso exige pergunta já materializada. (`app/escala/models.py:1735`; `app/chamados/juizes.py:1050`, `:1056`).

Base disponível em `/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e`; a cadeia abaixo usa os arquivos dessa base:

1. `registrar_batida` cria a batida → receiver do cartório → `_julgar_dias` → `julgar_celula` → `julgar_colab` → `_julgar_colab_corpo`. (`app/ponto/registro_batida.py:157`; `app/ponto/signals.py:132`, `:135`, `:93`; `app/ponto/services/cartorio.py:1560`, `:744`).
2. O corpo monta a ata nova, publica-a no contexto do julgamento e, com os marcos cobrados acesos, chama `x.reconciliar()`. (`app/ponto/services/cartorio.py:1148`, `:1152`, `:1163`).
3. Na base, `reconciliar` pede a decisão; o gate da premissa aceita `premissa_regularizada`; `_gravar_estado` atribui o terminal e salva. (`base/app/chamados/models.py:440`, `:470`, `:495`, `:444`, `:192`, `:231`).
4. **Após esse save e antes da segunda lavra, o chamado já está terminal e a pergunta sem carimbo continua viva pelo veredito anterior acusador.** C6 não possui veto geral por destino terminal antes desse ramo; o receiver do chamado evita julgamento recursivo enquanto o cartório está ativo. (`base/app/chamados/models.py:231`; `app/chamados/juizes.py:1050`, `:1054`, `:1056`; `app/chamados/signals.py:174`).
5. O fechamento enfileira a célula para nova passada; a segunda passada relê os chamados, agora sem aquele vivo. Sem códigos e sem cobrança, `derivar_veredito([])` produz `concorde`. (`app/ponto/services/cartorio.py:1166`, `:745`, `:746`, `:1069`, `:1409`; `app/escala/models.py:1596`).
6. O receiver posterior da batida chama `reconciliar_chamados_colab`, que começa por `retratar_perguntas_absolvidas_por_celula`; esse consumidor exclui células acusadoras, confirma C6 falso e grava `materializada_em`, usuário e `celula_absolveu`. (`app/ponto/signals.py:207`, `:215`; `app/chamados/reconciliador.py:1350`, `:598`, `:613`, `:617`, `:619`, `:620`).

Leitores diretos de `cobrado` / `VEREDITOS_ACUSAM`, fora de testes e migrations: [nao verificado]

- C6 e consultas de perguntas: `app/chamados/juizes.py:1002`, `:1056`; `app/chamados/models.py:2523`, `:2524`.
- Fechamento, emissão e fio: `app/chamados/reconciliador.py:598`, `:978`; `app/chamados/services/fabrica_da_celula.py:86`; `app/chamados/services/silencio_ausencia.py:41`; `app/chamados/services/resposta_tardia.py:45`; `app/chamados/services/limbo_folgas.py:194`; `app/chamados/services/disputa_emissao.py:705`.
- Folha e apresentação: `app/folha/export.py:1215`; `app/relatorios/dia_da_frota.py:13`, `:55`; `app/relatorios/furos.py:25`, `:46`; `app/colaboradores/services/calendario.py:70`, `:173`.
- Vigias/comandos: `app/chamados/services/furo_sem_canal.py:35`, `:72`; `app/chamados/services/vigia_fio.py:182`, `:230`; `app/folha/services/vigia_pausa.py:155`, `:157`; `app/ponto/services/vigia_ausencia_veredito.py:41`, `:42`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`, `:62`; `app/chamados/management/commands/disparar_perguntas_competencia.py:86`, `:120`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:133`; `app/chamados/management/commands/auditar_invariantes_chamados.py:197`, `:342`, `:1387`, `:1601`, `:1643`.

A chave de contador `cobrado` em `app/ponto/services/furos_sem_cobranca.py:131` não é leitura de `CelulaDia.veredito`.

## 30/10 19:52 · sessao 941ba3963a4b · - · - · origem=Ronald (tmux)

Q5 — o contador mede **perguntas carimbadas**, uma por retorno verdadeiro de `p.encerrar_fantasma(user)`; não mede chamados nem disputas fechados (`app/chamados/reconciliador.py:1094`, `app/chamados/reconciliador.py:1104`). A porta grava `via_resolucao='fantasma'`, `materializada_em` e autor; retorna `False` quando já estava fantasma e carimbada (`app/chamados/models.py:2435-2442`).

O universo varrido é todo o colaborador: perguntas sem materialização, sem validação, em disputa aberta, com data e motivo `orfao_14h` ou `saida_sem_entrada` (`app/chamados/reconciliador.py:991`, `app/chamados/reconciliador.py:995-1005`, `app/chamados/reconciliador.py:1082`). Para entrar no contador exige `veredito_orfa == ENCERRA`: resposta vazia, luz do próprio marco acesa, C6 vivo, dia passado e encerrado pelo juiz (`app/chamados/reconciliador.py:1033-1043`, `app/chamados/reconciliador.py:1083-1096`).

A `self.respondida` **não é a segunda pergunta**: a fixture lhe dá resposta `'06:55'`, e `veredito_orfa` retorna `None` antes de consultar C6 para resposta não vazia (`app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:90-93`; `app/chamados/reconciliador.py:1033-1038`). A contagem também é anterior às guardas para fechar a disputa; resposta pendente pode barrar disputa sem desfazer o incremento (`app/chamados/reconciliador.py:1104-1119`).

A mudança é no **estado que chega à varredura**, não na fórmula do contador: base `_decisao_do_fechamento` só consultava motor e premissa (`/home/ronald/codex_ws/e54eae6ed8c46bc652434308bf918485dd3ac23e/app/chamados/models.py:467-472`); cópia acrescenta plano/C6 e retorna `None` diante de viva sem reata (`app/chamados/models.py:601-604`), fazendo `reconciliar()` retornar `False` (`app/chamados/models.py:574-576`).

Há caminho de geração automática **antes das duas perguntas explícitas**: fixture cria batidas E e S antes de `self.muda` e `self.respondida` (`app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:70-93`); cada batida aciona cartório (`app/ponto/signals.py:131-135`), que chama a fábrica após julgar (`app/ponto/services/cartorio.py:754-772`); a fábrica gera disputa/perguntas (`app/chamados/services/fabrica_da_celula.py:118-124`, `app/chamados/services/fabrica_da_celula.py:143-149`).

Assim, a explicação estática é: o novo gate conserva uma pergunta automática que a base podia eliminar durante o setup, aumentando as candidatas posteriormente carimbadas. A identidade/PK e o motivo exatos da segunda pergunta contada não constam dos anexos: **[nao verificado]**. Não atribuir o `2` à respondida nem a duas disputas fechadas (`app/chamados/reconciliador.py:1033-1034`, `app/chamados/reconciliador.py:1104-1119`).

## 30/10 19:33 · sessao f393335dd52b · O261-V-FASE · verificador · origem=Code

**Achei um ponto funcional procedente: a CLI exige o vínculo “lido na listagem”, mas não o exibe.** O argumento é obrigatório e filtra corretamente a proposta; porém a impressão omite `vinculo_id`. Duas propostas iguais, de vínculos distintos, ficam indistinguíveis para quem precisa informar `--vinculo`. `app/escala/management/commands/propostas_de_escala.py:23`, `:33`, `:39`, `:80`.

**1. Quem escreve e quem lê o vínculo**

- **CLI:** o operador fornece `--vinculo`; o parser recebe, o filtro combina colaborador e vínculo, e aplicar/recusar recebem a proposta selecionada. Falta imprimir o EC na listagem. `app/escala/management/commands/propostas_de_escala.py:23`, `:39`, `:63`, `:68`, `:80`.
- **Fio:** `proposta_do_fio` fornece `vinculo_id`; os dois formulários escrevem o hidden; `processar_post_fio` lê e repassa nos dois verbos; o recálculo exige correspondência do vínculo. Não encontrei bypass nesse percurso. `app/chamados/services/proposta_no_fio.py:79`, `:109`, `:134`, `:169`; `app/templates/chamados/partials/modal_fio.html:52`, `:59`; `app/chamados/services/fio.py:509`, `:511`, `:514`; `app/escala/services/propositor.py:685`.
- **Chat:** o preview guarda o vínculo na sessão; o aceite o recupera e filtra junto da assinatura. Sessão antiga sem essa chave passa `''`, que não corresponde ao EC. `app/core/views_ia.py:578`, `:620`, `:650`; `app/escala/services/propositor.py:685`.
- **Escrita final:** a porta recebe o vínculo da proposta; decisões aceita e recusada também o registram. `app/escala/services/propositor.py:718`, `:766`, `:774`.

Ocorrência desses percursos em produção: **[nao verificado]**.

**2. Imports, código morto e referências antigas**

Não encontrei import local inexistente na análise estática dos `from` absolutos de `app`; execução real e cobertura dos imports relativos: **[nao verificado]**. Os candidatos sem definição estática `ENVELOPE_LOTE_MIN` e `ROTULO_LOTE` existem via `__getattr__`. `app/chamados/services/validacao.py:765`.

Há parâmetros sem leitura nos corpos da cópia:

- `empresas` em `_fase_deslocada`, `_termometro`, `_vinculo_divergente` e `_teto_do_dia`. O filtro efetivo da lista ocorre depois. `app/escala/services/cadastro_realidade.py:335`, `:285`, `:303`, `:315`, `:635`.
- `dias_bat` e `dias_janela` em `_propor_pausa`; `dias_bat` em `_chamados_que_morreriam`; `hoje` em `_recusadas_recentes`. `app/escala/services/propositor.py:280`, `:617`, `:693`.
- `hoje` em `linhas_para_a_pauta`. `app/chamados/services/fila_por_causa.py:379`.

A introdução desses resíduos **nesta obra** é **[nao verificado]**. Os parâmetros com `_` de `dono_do_dia` estão explicitamente reservados à decisão de lei aberta. `app/escala/services/fase_12x36.py:78`, `:88`.

Não encontrei teste importando/executando os antigos `fora_de_fase/JANELA_DIAS/PISO/CICLOS` de `fase_12x36`. A referência executável restante a `fora_de_fase` verifica sua ausência; outras são lápides/documentação. `app/escala/tests/test_o261_v_fase_na_lista.py:223`; `app/escala/tests/test_fase_12x36.py:3`; `app/chamados/tests/test_proposta_no_fio.py:186`.

**3. Os testes mordem o chamador?** [nao verificado]

**Sim, no repasse do POST:** exercitam `processar_post_fio` para aplicar e recusar e exigem `vinculo_da_tela='9'`. O render exige os dois hidden. `app/escala/tests/test_o261_v_fase_na_lista.py:434`, `:438`, `:439`; `app/chamados/tests/test_proposta_faixa_na_tela.py:85`, `:91`.

O limite é preciso: o POST substitui os serviços por mocks; o cenário A desaparece/B tem mesma assinatura e seu controle positivo estão no auxiliar `_recalcular`. `app/escala/tests/test_o261_v_fase_na_lista.py:437`, `:418`, `:422`, `:425`.

A CLI é exercitada por `call_command`, mas somente para aplicar sem vínculo. Não verifica a impressão do EC nem a seleção positiva — deixou escapar o achado da listagem. `app/escala/tests/test_o261_v_fase_na_lista.py:444`, `:451`, `:453`.

Os intervalos históricos exatos são **[nao verificado]**, pois a cópia não contém `.git`. Execução da suíte Django: **[nao verificado]**. [nao verificado]

## 30/10 19:32 · sessao 6430dea1929f · - · - · origem=Ronald (tmux)

Achado de limpeza: `empresas` não é lido em `_fase_deslocada(empresas=None)`; o chamador passa esse argumento, mas o corpo apenas lê/formata a lavra. O filtro efetivo por empresa ocorre depois, em `lista()`. Isso não demonstra vazamento de escopo. Origem no intervalo `d80c98a5..2177fa08`: [nao verificado]. `app/escala/services/cadastro_realidade.py:335`, `:337`, `:348`, `:610`, `:635`.

Outros parâmetros sem leitura na cópia, cuja origem histórica é [nao verificado]:

- `_propor_pausa`: `dias_bat`, `dias_janela`. `app/escala/services/propositor.py:280`.
- `_chamados_que_morreriam`: `dias_bat`. `app/escala/services/propositor.py:617`.
- `_recusadas_recentes`: `hoje`; corte usa `timezone.now()`. `app/escala/services/propositor.py:693`, `:700`.
- `_termometro`, `_vinculo_divergente`, `_teto_do_dia`: `empresas`. `app/escala/services/cadastro_realidade.py:285`, `:303`, `:315`.
- `linhas_para_a_pauta`: `hoje`. `app/chamados/services/fila_por_causa.py:379`.

Os argumentos `_colab_id`, `_dia`, `_ec_do_dia` de `dono_do_dia` são explicitamente reservados à decisão de lei aberta; não classifiquei como resíduo acidental. `app/escala/services/fase_12x36.py:78`, `:81`, `:88`.

Não encontrei import local inexistente na varredura AST dos `from` absolutos locais de `app`; execução real desses imports: [nao verificado]. Os únicos nomes sem definição estática encontrados (`ENVELOPE_LOTE_MIN`, `ROTULO_LOTE`) são fornecidos por `__getattr__`. `app/chamados/services/validacao.py:765`, `:767`, `:769`.

Não encontrei teste executando/importando `fora_de_fase`, `JANELA_DIAS`, `PISO` ou `CICLOS` de `fase_12x36`. A referência executável restante a `fora_de_fase` exige sua ausência; demais ocorrências são comentários/lápides. `app/escala/tests/test_o261_v_fase_na_lista.py:223`, `:226`; `app/escala/tests/test_fase_12x36.py:3`; `app/chamados/tests/test_proposta_no_fio.py:186`.

Todas as funções/constantes de topo de `fase_12x36`, `propositor`, `censo_fase_12x36` e `proposta_no_fio` tiveram referência AST de leitura/importação encontrada; isso não prova alcance em produção. [nao verificado].

## 30/10 19:32 · sessao 92ffc021dfdb · - · - · origem=Ronald (tmux)

Achado procedente: **a CLI exige um vínculo que sua listagem não informa**. `--vinculo` é anunciado como “o vínculo da proposta que você leu na listagem” e obrigatório para decidir (`app/escala/management/commands/propostas_de_escala.py:23`, `:33`), mas o loop imprime somente colaborador, template, âncoras, casamento e chamados; nunca `vinculo_id` (`:80`, `:83`, `:89`, `:93`). Duas propostas iguais exceto pelo vínculo aparecem indistinguíveis. Reprodução somente em memória com a classe `Command` e propostas fictícias confirmou isso; ocorrência concreta em produção [nao verificado].

Fluxo conferido:

- CLI: humano fornece `--vinculo`; parser lê em `propostas_de_escala.py:23`; filtro seleciona por colaborador **e** vínculo em `:39`; aplicação/recusa recebem essa proposta em `:63`, `:68`. Falta o escritor do ID na listagem (`:80`).
- Fio: `proposta_do_fio` fornece o ID em `app/chamados/services/proposta_no_fio.py:79`; os dois formulários escrevem o hidden em `app/templates/chamados/partials/modal_fio.html:52`, `:59`; POST lê `''` por omissão e repassa em `app/chamados/services/fio.py:509`, `:511`, `:514`; os serviços passam ao recálculo em `proposta_no_fio.py:134`, `:169`; o auxiliar compara o vínculo em `app/escala/services/propositor.py:685`. Não identifiquei bypass nesse caminho.
- Chat: preview guarda o vínculo em sessão em `app/core/views_ia.py:620`; aceite lê com `''` por omissão em `:650`; helper repassa à seleção por assinatura/vínculo em `:578`. Não identifiquei bypass nesse caminho.
- Escrita final usa `p['vinculo_id']` na porta em `app/escala/services/propositor.py:766`; decisão aceita e recusada registram o ID em `:774`, `:718`.
- Caminhos produtivos encontrados são CLI, fio e chat: chamadas diretas em `propostas_de_escala.py:63`, `:68`, `proposta_no_fio.py:147`, `:172`, `views_ia.py:660`; escritor dinâmico declarado em `app/core/intake.py:51`.

Testes relacionados: POST exercita o chamador real nos dois verbos e confere o repasse (`app/escala/tests/test_o261_v_fase_na_lista.py:428`, `:434`, `:439`); render exige dois hidden com o ID (`app/chamados/tests/test_proposta_faixa_na_tela.py:85`, `:91`). Teste CLI cobre ausência de vínculo somente no aplicar (`test_o261_v_fase_na_lista.py:444`, `:451`); não cobre sua impressão na listagem nem seleção positiva do EC (`:448`).

Atribuição exata ao delta histórico [nao verificado]: a cópia disponibilizada não contém `.git`.

## 30/10 19:32 · sessao de6d5691a76e · - · - · origem=Ronald (tmux)

Achado procedente: a CLI exige o EC “lido na listagem”, mas não o imprime. `--vinculo` é definido e exigido em `app/escala/management/commands/propostas_de_escala.py:23` e `:33`; a seleção usa `vinculo_id` em `:39`. Porém a listagem em `:80`–`:98` imprime colaborador, tipo, templates, âncoras, data e chamados, sem EC. Duas propostas iguais exceto pelo vínculo produzem blocos indistinguíveis; confirmei executando `Command.handle` extraído por AST em memória, com módulos falsos e `python3 -B`.

Os testes mordem estes chamadores: [nao verificado]

- `app/escala/tests/test_o261_v_fase_na_lista.py:434` percorre aplicar e recusar, chama `processar_post_fio` em `:438` e exige o repasse `vinculo_da_tela='9'` em `:439`. Tirar o repasse em `app/chamados/services/fio.py:511` ou `:514` quebra a asserção.
- `app/chamados/tests/test_proposta_faixa_na_tela.py:85` renderiza a faixa e exige dois hidden com o vínculo em `:91`; os campos ficam nos dois formulários em `app/templates/chamados/partials/modal_fio.html:52` e `:59`.
- O cenário A sumiu/B mesma assinatura testa o auxiliar `_recalcular`: rejeita vínculo A em `app/escala/tests/test_o261_v_fase_na_lista.py:418`, rejeita vínculo vazio em `:422` e aceita B como controle positivo em `:425`.
- A CLI é chamada de verdade por `call_command` em `app/escala/tests/test_o261_v_fase_na_lista.py:451`; o teste exige nenhuma aplicação e mensagem `--vinculo` em `:452` e `:453`.

Limites: o teste HTTP substitui os serviços de aplicar/recusar por mocks (`app/escala/tests/test_o261_v_fase_na_lista.py:437`), portanto verifica o repasse do chamador, não a cadeia HTTP até o escritor. O controle positivo está no auxiliar, não no POST inteiro (`:424`). O teste CLI cobre aplicar sem vínculo, sem verificar a saída da listagem nem a seleção positiva (`:444`). Isso deixou escapar a ausência do EC na listagem.

Suite Django executada: [nao verificado]. Atribuição exata desses trechos ao delta, sem `.git`: [nao verificado]. [nao verificado]

## 30/10 19:28 · sessao eb56abb9e8b6 · O261-V-FASE · verificador · origem=Code

A cura impede A→B no fio e no chat quando o vínculo original acompanha a decisão. Restam lacunas nos testes e caminhos sem identidade do preview na CLI e entre abas do chat. (`app/escala/services/propositor.py:682`, `app/chamados/services/fio.py:511`, `app/core/views_ia.py:650`, `app/escala/management/commands/propostas_de_escala.py:26`, `app/core/views_ia.py:871`)

Comparação literal de `ed31456f..23c0d9dc`: [não verificado], pois a cópia disponibilizada não contém `.git`. A atribuição dos residuais abaixo ao delta, em vez de comportamento anterior, fica [não verificado].

**1. Quem escreve e quem lê**

| Caminho | Escrita/origem e leitura do vínculo |
|---|---|
| Fio: produção e template | `proposta_do_fio` escolhe `p` e extrai dela assinatura e vínculo; os formulários de aplicar e recusar escrevem ambos em hidden. (`app/chamados/services/proposta_no_fio.py:66`, `app/chamados/services/proposta_no_fio.py:75`, `app/chamados/services/proposta_no_fio.py:79`, `app/templates/chamados/partials/modal_fio.html:51`, `app/templates/chamados/partials/modal_fio.html:58`) |
| Fio: POST e serviços | `fio.py` lê `vinculo_proposta`, usa `''` quando ausente e repassa aos dois verbos; ambos chegam a `_recalcular`, que passa o vínculo a `da_assinatura`. (`app/chamados/services/fio.py:509`, `app/chamados/services/fio.py:511`, `app/chamados/services/fio.py:513`, `app/chamados/services/proposta_no_fio.py:134`, `app/chamados/services/proposta_no_fio.py:169`, `app/chamados/services/proposta_no_fio.py:109`) |
| Chat: preview e sessão | O preview guarda assinatura e `p.get('vinculo_id')` na sessão e monta o texto com essa mesma `p`. O “sim” lê a proposta da sessão; `_aplicar_escala` repassa seu vínculo para `_proposta_de_escala`, que chama `da_assinatura`. (`app/core/views_ia.py:620`, `app/core/views_ia.py:623`, `app/core/views_ia.py:871`, `app/core/views_ia.py:650`, `app/core/views_ia.py:578`) |
| Auxiliar | `da_assinatura` filtra por pessoa, assinatura e vínculo; `None` desliga o filtro de vínculo. (`app/escala/services/propositor.py:682`) |
| Escrita final | Aplicar entrega `p['vinculo_id']` à porta; aceite e recusa gravam o vínculo em `DecisaoProposta`. (`app/escala/services/propositor.py:766`, `app/escala/services/propositor.py:774`, `app/escala/services/propositor.py:718`) |
| CLI | Não recebe assinatura nem vínculo da listagem: recalcula, filtra pelo colaborador, recusa multiplicidade e decide sobre a proposta única atual. (`app/escala/management/commands/propostas_de_escala.py:11`, `app/escala/management/commands/propostas_de_escala.py:26`, `app/escala/management/commands/propostas_de_escala.py:30`, `app/escala/management/commands/propostas_de_escala.py:52`) |

**2. Falhas, contraexemplos e descartes**

- **Residual procedente — sessão compartilhada entre abas.** Aba 1 mostra A; aba 2 gera preview B e sobrescreve `proposta_ia`; o usuário volta à aba 1 e envia “sim”. O POST não identifica o preview: envia pergunta/histórico/CSRF. A view consome B da sessão, valida B e aplica B. Portanto, o vínculo pode diferir daquele do preview ainda visível na aba que confirmou. (`app/core/views_ia.py:88`, `app/core/views_ia.py:620`, `app/templates/core/_copiloto.html:172`, `app/core/views_ia.py:106`, `app/core/views_ia.py:871`, `app/core/views_ia.py:650`, `app/core/views_ia.py:660`) Ocorrência em produção: [não verificado].

- **Residual procedente — CLI sem vínculo da leitura anterior.** Contraexemplo: a listagem mostrou A; A desaparece e sobra B única; `--aplicar COLAB` aplica B, ou `--recusar COLAB` recusa B. A guarda cobre propostas simultâneas, mas não liga a decisão à listagem anterior. (`app/escala/management/commands/propostas_de_escala.py:26`, `app/escala/management/commands/propostas_de_escala.py:31`, `app/escala/management/commands/propostas_de_escala.py:38`, `app/escala/management/commands/propostas_de_escala.py:53`, `app/escala/management/commands/propostas_de_escala.py:58`)

- **`str` × `int`: descartado.** `'1'` do formulário casa com `1` da proposta porque os dois lados passam por `str`. Tela antiga sem campo e sessão antiga sem chave viram `''`, que não casa com IDs normais. (`app/escala/services/propositor.py:685`, `app/chamados/services/fio.py:509`, `app/core/views_ia.py:651`)

- **`None` permanece uma abertura no auxiliar.** Contraexemplo: A some, sobra B com a mesma assinatura e o chamador omite o vínculo; `None` desliga o filtro e B é selecionada. Os chamadores HTTP atuais usam `''` para campo/chave ausente, evitando esse caso. (`app/escala/services/propositor.py:668`, `app/escala/services/propositor.py:685`, `app/chamados/services/fio.py:509`, `app/core/views_ia.py:651`)

- **Proposta sem `vinculo_id`: fragilidade condicional, sem origem normal demonstrada.** O gerador sempre acrescenta `ec.pk`. Se chegasse uma proposta incompleta ao preview do chat, ele guardaria `None`, posteriormente desativando o filtro; o texto `'None'` também casa com um ID ausente no auxiliar. Isso não demonstra falha produtiva, pois contradiz a saída normal do gerador. (`app/escala/services/propositor.py:539`, `app/core/views_ia.py:622`, `app/core/views_ia.py:651`, `app/escala/services/propositor.py:685`) Ocorrência desse payload: [não verificado].

- **Recusa/L-042: vínculo gravado, silêncio preservado por conteúdo.** Contraexemplo: A e B da mesma pessoa têm a mesma assinatura; recusar A também cala B por 30 dias. A consulta e o filtro usam apenas `(colaborador, assinatura)`. O comentário declara expressamente a preservação dessa chave; não classifico isso como omissão acidental do transporte. (`app/escala/services/propositor.py:718`, `app/escala/services/propositor.py:663`, `app/escala/services/propositor.py:671`, `app/escala/services/propositor.py:700`, `app/escala/services/propositor.py:581`) No chat, texto diferente de “sim” remove a pendência, sem executar a recusa formal de escala. (`app/core/views_ia.py:882`)

**3. Os testes mordem os chamadores?** [nao verificado]

**Para o vínculo deste delta, não.** O novo “A some/B fica” chama diretamente `_recalcular`, com `'1'` e `''`; não atravessa formulário, `fio.py`, aplicar/recusar ou sessão do chat. Remover o repasse em `fio.py` não seria detectado por esse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:408`, `app/escala/tests/test_o261_v_fase_na_lista.py:418`, `app/escala/tests/test_o261_v_fase_na_lista.py:422`, `app/chamados/services/fio.py:511`, `app/chamados/services/fio.py:514`)

O selo também carece de controle positivo com vínculo informado: uma implementação que recusasse **todo vínculo não nulo** satisfaria os dois casos negativos; o teste anterior chama sem vínculo. (`app/escala/tests/test_o261_v_fase_na_lista.py:404`, `app/escala/tests/test_o261_v_fase_na_lista.py:418`, `app/escala/tests/test_o261_v_fase_na_lista.py:422`)

Há testes HTTP anteriores do chat, mas cobrem proposta única, conteúdo alterado e desaparecimento. O selo novo do chat verifica apenas estreitamento por colaborador; o de renderização do fio verifica assinatura, sem afirmar o hidden de vínculo. (`app/core/tests/test_ia_escala_com_sim.py:49`, `app/core/tests/test_ia_escala_com_sim.py:86`, `app/core/tests/test_ia_escala_com_sim.py:98`, `app/core/tests/test_ia_escala_com_sim.py:111`, `app/escala/tests/test_o261_v_fase_na_lista.py:429`, `app/chamados/tests/test_proposta_faixa_na_tela.py:85`)

Execução da suíte Django e resultados em produção: [não verificado]. [nao verificado]

## 30/10 19:27 · sessao f0119dee472e · - · - · origem=Ronald (tmux)

- **Tipos:** `da_assinatura` compara `str(p.get('vinculo_id')) == str(vinculo_id)`, portanto POST `'1'` casa com ID inteiro `1`; reproduzi em memória, sem Django/BD. `None` desliga o filtro; `''` recusa IDs válidos. (`app/escala/services/propositor.py:682-690`)

- **CLI continua sem vínculo do que foi lido:** argumentos aceitam apenas colaborador; a execução recalcula e pega a proposta única atual. Contraexemplo: listou A, A desaparece, sobra B única; `--aplicar COLAB` aplica B; `--recusar COLAB` recusa B. A guarda só cobre duas propostas simultâneas, para ambas as ações. Isso é caminho residual no código atual; inclusão no delta é [não verificado]. (`app/escala/management/commands/propostas_de_escala.py:14-22,26-38,52-58`)

- **Recusa e silêncio:** a decisão grava o vínculo, mas a chave do silêncio continua `(colaborador_id, assinatura)` por 30 dias; assinatura exclui vínculo. Contraexemplo: A e B, mesma pessoa e conteúdo; recusar A cala B também. O próprio comentário declara essa preservação; não classificaria como regressão acidental do delta. (`app/escala/services/propositor.py:653-665,671-678,700-702,717-720,581-583`)

- **Proposta sem ID:** o gerador sempre acrescenta `vinculo_id=ec.pk`; não há evidência de saída real sem ID. No auxiliar, `vinculo_id=None` aceita proposta sem chave; `''` a recusa. `recusar` tolera FK nula; `aplicar` exige `p['vinculo_id']`. (`app/escala/services/propositor.py:539-543,685,718,766`)

- **Selo sem controle positivo:** o teste novo chama somente `_recalcular`, em dois casos negativos: A sumiu e tela antiga `''`. Uma mutação que recuse todo `vinculo_da_tela` não nulo passaria esses casos; o teste anterior chama sem vínculo. Não prova `fio.py`, formulário, nem sessão do chat. (`app/escala/tests/test_o261_v_fase_na_lista.py:395-423`)

- **Chat:** o teste ali somente verifica estreitamento por colaborador. Existem testes HTTP anteriores de confirmação, conteúdo diferente e desaparecimento, mas aqueles casos não exercitam troca A→B com mesma assinatura. (`app/escala/tests/test_o261_v_fase_na_lista.py:429-434`; `app/core/tests/test_ia_escala_com_sim.py:86-118`)

- **Testes de silêncio:** exercitam recusa, expiração e assinatura diferente com um EC; não cobrem A/B com mesma assinatura. (`app/escala/tests/test_porta_da_proposta.py:77-80,104-128`)

## 30/10 19:27 · sessao 77fbbcca86ad · - · - · origem=Ronald (tmux)

O chat transporta o vínculo da mesma proposta usada no preview: seleção em `app/core/views_ia.py:580`, gravação na sessão em `app/core/views_ia.py:620`, texto formado dessa proposta em `app/core/views_ia.py:623`, leitura no “sim” em `app/core/views_ia.py:650` e filtro em `app/escala/services/propositor.py:685`.

Há residual com duas abas: a sessão usa uma única chave (`app/core/views_ia.py:88`), sobrescrita a cada preview (`app/core/views_ia.py:620`). Contraexemplo: aba 1 mostra A; aba 2 produz B; “sim” digitado na aba 1 envia apenas pergunta/histórico/CSRF (`app/templates/core/_copiloto.html:172`), e a view consulta a proposta B da sessão (`app/core/views_ia.py:106`, `app/core/views_ia.py:871`), valida B (`app/core/views_ia.py:650`) e aplica B (`app/core/views_ia.py:660`). O vínculo recebido pode, portanto, diferir do preview ainda visível na aba que confirmou; origem no delta versus comportamento herdado e ocorrência produtiva: [não verificado].

Sessão antiga sem a chave `vinculo` recebe `''` (`app/core/views_ia.py:651`), que não casa IDs normais (`app/escala/services/propositor.py:685`). Sessão contendo explicitamente `None` desliga o filtro nesse mesmo ponto; o produtor normal injeta `ec.pk` (`app/escala/services/propositor.py:539`), portanto ocorrência desse payload incompleto: [não verificado].

Os testes existentes mordem a rota de chat, mas com uma proposta de vínculo 7 (`app/core/tests/test_ia_escala_com_sim.py:49`, `app/core/tests/test_ia_escala_com_sim.py:86`), conteúdo alterado (`app/core/tests/test_ia_escala_com_sim.py:98`) ou lista vazia (`app/core/tests/test_ia_escala_com_sim.py:111`). O teste novo de A sumir/B conservar assinatura chama apenas `_recalcular`, sem passar pelo chat ou pela sessão (`app/escala/tests/test_o261_v_fase_na_lista.py:408`). O selo do chat verifica apenas o estreitamento por colaborador (`app/escala/tests/test_o261_v_fase_na_lista.py:429`).

Texto diferente de “sim” apaga a proposta pendente (`app/core/views_ia.py:882`); esse caminho não registra recusa de escala em `DecisaoProposta`, cuja escrita está em `app/escala/services/propositor.py:705`.

## 30/10 19:27 · sessao 83aa580b3dfe · - · - · origem=Ronald (tmux)

Fluxo do fio conferido:

- `proposta_do_fio` obtém propostas da pessoa, seleciona `[0]` e produz tanto assinatura quanto vínculo desse mesmo objeto (`app/chamados/services/proposta_no_fio.py:63-79`).
- O template grava assinatura e vínculo nos dois formulários, aplicar e recusar (`app/templates/chamados/partials/modal_fio.html:48-59`).
- `processar_post_fio` lê o vínculo do POST como string, usa `''` quando ausente e repassa aos dois serviços (`app/chamados/services/fio.py:505-514`).
- Aplicar e recusar chamam `_recalcular` com o vínculo recebido; este passa o argumento a `da_assinatura` (`app/chamados/services/proposta_no_fio.py:109-110`, `:134`, `:169`).
- `da_assinatura` compara os vínculos convertidos para `str`; portanto `'1'` do POST casa com `1` da proposta. O vínculo ausente do POST fica `''` e não casa com IDs reais (`app/escala/services/propositor.py:682-690`).

Não achei desvio natural no transporte do vínculo pelo fio: os formulários e os dois verbos usam o vínculo da proposta apresentada (`app/templates/chamados/partials/modal_fio.html:51-59`, `app/chamados/services/fio.py:507-514`, `app/chamados/services/proposta_no_fio.py:75-79`).

Limites relevantes:

1. **O filtro continua opcional no auxiliar.** `vinculo_id=None` desliga a restrição. Contraexemplo: `_recalcular(colab, assinatura_de_A)` com apenas B restante, mesma assinatura, devolve B. Os chamadores HTTP atuais passam `''` quando o campo falta, evitando esse caso; o teste anterior ainda exercita a API sem vínculo (`app/escala/services/propositor.py:685`, `app/chamados/services/fio.py:509`, `app/escala/tests/test_o261_v_fase_na_lista.py:403-406`).

2. **Proposta sem vínculo não recebe validação própria.** Um vínculo textual `'None'` casa com `p.get('vinculo_id') is None`; `None` como argumento desliga o filtro. Porém o produtor real preenche `vinculo_id=ec.pk` em toda proposta antes de retorná-la. Uma proposta real sem vínculo não foi demonstrada pelo código ou anexo (`app/escala/services/propositor.py:539-543`, `:685`) [não verificado].

3. **A recusa continua silenciando por conteúdo, atravessando vínculos.** Recusar A persiste a FK de A, mas a consulta e o filtro de silêncio usam somente `(colaborador, assinatura)`. Contraexemplo: A/B da mesma pessoa, mesmo conteúdo; recusar A também cala B por trinta dias. Isso está explicitamente preservado no comentário do delta, que mantém a chave de L-042; não é omissão acidental do transporte (`app/escala/services/propositor.py:581-583`, `:663-678`, `:693-702`, `:717-720`).

**Os testes não mordem o chamador do fio para este delta.** O novo RED chama `_recalcular` diretamente com `'1'` e `''`; não atravessa template, POST, `aplicar_do_fio` ou `recusar_do_fio`. Remover `vinculo_da_tela=_vinc` de `fio.py` não seria detectado por esse RED (`app/escala/tests/test_o261_v_fase_na_lista.py:408-423`, `app/chamados/services/fio.py:511-514`). O teste de render existente afirma apenas que a assinatura aparece duas vezes, sem verificar os campos de vínculo (`app/chamados/tests/test_proposta_faixa_na_tela.py:85-89`). Os testes antigos dos serviços também chamam aplicar/recusar sem vínculo, utilizando o comportamento opcional (`app/chamados/tests/test_proposta_no_fio.py:118`, `:175`).

## 30/10 19:23 · sessao 7163f16a20d4 · O261-V-FASE · verificador · origem=Code

**Há pendências procedentes: a troca de vínculo preservando a assinatura ainda passa, existem dois caminhos de frota dentro de requests e os testes novos não cobrem integralmente os consumidores.** (`app/escala/services/propositor.py:663–684`; `app/chamados/services/proposta_no_fio.py:133–171`; `app/api/views_mensageria.py:2019,2207`; `app/escala/tests/test_o261_v_fase_na_lista.py:387–409`)

Comparação efetiva dos commits, execução da suíte, RED histórico e ocorrência em produção: **[nao verificado]**. As conclusões abaixo referem-se ao código presente. [nao verificado]

1. **Quem escreve e quem lê**

   `JUIZES.md` distingue vínculo do dia, cobertura da data, dia de trabalho e geometria de turno; também esclarece que é um índice, enquanto o registro vivo está em `core/juizes.py`. O juiz e o escritor da proposta estão declarados no intake. (`app/docs/JUIZES.md:7–12,20–25,66–78`; `app/core/intake.py:49–53`)

   | Valor | Produção/escrita | Consumidores |
   |---|---|---|
   | `dono_do_dia` da lavra | `fase_12x36.dono_do_dia` devolve `'CADASTRO'`; o censo chama esse ponto por item; `lavrar` persiste o snapshot com `update_or_create`. (`app/escala/services/fase_12x36.py:78–100`; `app/ponto/management/commands/censo_fase_12x36.py:250–265`) | `_fase_deslocada` copia para C10; `lista` expõe o dono e conta ocorrências por dono; tela/PDF apresentam a assinatura. (`app/escala/services/cadastro_realidade.py:335–347,647–658,784`; `app/templates/escala/cadastro_x_realidade.html:28`) |
   | Retorno de `dias_na_lista` | Monta `{(colab_id, data): dono ou None}`; `_dono_resolvido` prioriza C9 e devolve `None` se houver conflito no nível escolhido. (`app/escala/services/cadastro_realidade.py:706–742`) | `vigia_fio` consulta o mapa: somente BATIDA/CADASTRO permitem encaminhar o dia para `fio_do_admin`, deixando de contá-lo em `fio_morto`. (`app/chamados/services/vigia_fio.py:255–266,314–324`) |
   | Retorno de `_recalcular` | Recalcula propostas da pessoa e procura a assinatura recebida; devolve `(p, erro)`. (`app/chamados/services/proposta_no_fio.py:92–116`) | `aplicar_do_fio` entrega `p` ao escritor do intake; `recusar_do_fio` entrega `p` a `propositor.recusar`. Ambos recebem a ação pelo POST do fio. (`app/chamados/services/proposta_no_fio.py:133–146,168–171`; `app/chamados/services/fio.py:505–512`) |
   | Retorno de `_proposta_de_escala` | Chama `propostas(colaborador_id=colab_pk)`; no preview escolhe a primeira da pessoa; com assinatura procura a correspondente. (`app/core/views_ia.py:562–583`) | `_preview_escala` monta o texto e guarda colaborador/assinatura na sessão; `_aplicar_escala` recalcula com essa assinatura e chama o escritor. (`app/core/views_ia.py:591,620–622,649–658`) |

   A aplicação usa a porta de correção retroativa e grava a decisão; a recusa grava `DecisaoProposta`. Não encontrei nesses caminhos um segundo escritor de correção de escala. (`app/escala/services/propositor.py:711–714,759–770`)

   Os demais leitores da lavra de fase usam seus itens ou conjuntos de colaboradores: propositor, fila por causa, pauta, API de fase e comando de vigia da família. (`app/escala/services/propositor.py:490–493`; `app/chamados/services/fila_por_causa.py:174`; `app/escala/services/fase_12x36.py:132–139`; `app/api/views_mensageria.py:1671–1672`; `app/ponto/management/commands/vigiar_familia_escala.py:22–33`)

2. **Falhas e pré-requisitos**

   **A — A-residual continua: a assinatura identifica conteúdo, mas não o vínculo alvo.** Ela inclui apenas tipo, template, âncora e início de aplicação. `da_assinatura` rejeita duas correspondências simultâneas, mas aceita uma correspondência sobrevivente de outro vínculo. (`app/escala/services/propositor.py:663–684`)

   **Contraexemplo:** o preview mostrou A/ec1; antes da confirmação A deixa de ser proposta; resta B/ec2 com os mesmos quatro campos. A assinatura continua igual; `_recalcular` devolve B sem erro; aplicar escreve B e recusar registra a recusa em B. O chat guarda somente colaborador/assinatura e também pode aplicar B. (`app/escala/services/propositor.py:663–684`; `app/chamados/services/proposta_no_fio.py:109–116,133–146,168–171`; `app/core/views_ia.py:620–622,649–658`)

   Isso contraria a proteção declarada de exigir a proposta efetivamente lida antes da decisão. O recálculo exigido pela L-042 ocorre, mas a identidade do alvo permanece insuficiente. (`app/chamados/services/proposta_no_fio.py:95–99`; `app/core/views_ia.py:651–656`; `app/docs/LEIS.md:90`)

   **B — Custo de frota no request permanece em duas APIs.** O primeiro caminho chama diretamente `propostas()`; o segundo monta a revisão semanal, que chama a pauta do propositor e termina em `propostas(hoje)` sem filtro. Sem `colaborador_id`, o propositor carrega os colaboradores da frota. (`app/api/views_mensageria.py:2019,2205–2207`; `app/escala/services/fase_12x36.py:174`; `app/escala/services/propositor.py:428–429,640`)

   **Contraexemplo:** um GET autenticado de propostas executa o cálculo da frota antes de limitar a resposta a oito itens; o teto da apresentação não limita o trabalho realizado. Tempo e consultas atuais: **[nao verificado]**. (`app/api/views_mensageria.py:2002,2016–2021`)

   **C — Pré-requisito aberto: ponto único de gravação não resolve o juiz do dono.** O wrapper continua devolvendo CADASTRO, enquanto a L-099 nomeia `e6_oraculo.dono_da_divergencia`; o próprio wrapper registra expressamente a pergunta aberta. Não considero isso uma cura concluída nem uma descoberta nova desta volta. (`CLAUDE.md:311–316`; `app/escala/services/fase_12x36.py:81–89`)

   **Contraexemplo de contrato:** sem flags especiais, contagem par e ausência na lista do motor, o e6 devolve ESTRUTURA; uma C10 isolada recebe CADASTRO e pode encaminhar o dia para fora do alarme. A ocorrência concreta dessa combinação em produção está **[nao verificado]**. (`app/ponto/management/commands/e6_oraculo.py:90–100`; `app/escala/services/fase_12x36.py:89`; `app/chamados/services/vigia_fio.py:266,314–324`)

   **D — Declaração do cron e selo não acompanham a prosa.** A prosa chama o censo de juiz único, mas ele continua cadastrado como `lavra`; `juizes_por_varredura` conta somente papel `juiz`. Assim, o censo reconhecido como juiz fica fora desse contador. O selo verifica a declaração, sem confrontá-la com o julgamento executado. (`app/config/crons.py:600–605,2145–2151,2246–2255`; `app/ponto/management/commands/censo_fase_12x36.py:227`; `app/core/tests/test_contract_tabuleiro.py:15–28`)

   Isso é inconsistência do cadastro/medição de papéis; não fundamento para desfazer a escolha específica do censo como autoridade da fase. (`CLAUDE.md:275–276`; `app/escala/tests/test_o261_v_fase_na_lista.py:4–8`)

   **Não achei uma derivação paralela nova em `dias_na_lista`:** ela seleciona donos já lavrados, priorizando a C9 proveniente do juiz; não reconstrói os insumos do dia. (`app/escala/services/cadastro_realidade.py:706–719`; `app/ponto/services/teto_na_lista.py:72–86`; `app/ponto/management/commands/e6_oraculo.py:146–155`)

3. **Outros chamadores: primeira proposta e frota**

   - **Decisão direta pela primeira sem guarda:** não encontrei outro caso nos caminhos examinados. A CLI usa `_dele[0]` somente depois de rejeitar mais de uma proposta. (`app/escala/management/commands/propostas_de_escala.py:30–38,52–58`)
   - **Apresentação ainda escolhe a primeira:** fio, ficha e preview do chat. Isso não constitui, por si só, outro aplicador direto. (`app/chamados/services/proposta_no_fio.py:66`; `app/colaboradores/services/ficha.py:176–189`; `app/core/views_ia.py:580–582`)
   - **Requests de frota encontrados:** `/mensageria/propostas-escala/` e `/mensageria/revisao-semanal/`, pelos caminhos descritos acima. (`app/api/urls.py:55,58`; `app/api/views_mensageria.py:2019,2207`; `app/escala/services/propositor.py:640`)
   - **Recortados:** painel pelos IDs da página com `so_existencia=True`; ficha por uma pessoa; fio e chat por uma pessoa. (`app/chamados/services/painel.py:518–522`; `app/colaboradores/services/ficha.py:176`; `app/chamados/services/proposta_no_fio.py:63,102`; `app/core/views_ia.py:575`)

4. **Os testes mordem o chamador? Parcialmente.** [nao verificado]

   | Cura retirada | O que o teste existente detecta | Limite | [nao verificado]
   |---|---|---|
   | Chamada ao ponto único do dono | **Sim:** substitui o helper por sentinela, executa o censo e exige a sentinela persistida. Voltar ao literal CADASTRO quebra essa asserção. (`app/escala/tests/test_o261_v_fase_na_lista.py:337–345`) | Não certifica qual juiz deve responder o dono. (`app/escala/tests/test_o261_v_fase_na_lista.py:338–339`) |
   | Independência da ordem | **Sim, em `dias_na_lista`:** testa C9/BATIDA e C10/CADASTRO nas duas ordens. (`app/escala/tests/test_o261_v_fase_na_lista.py:319–331`) | Não percorre o vigia com conflito. BATIDA e CADASTRO levam ao mesmo ramo; falta C9/ESTRUTURA + C10/CADASTRO permanecendo no alarme nas duas ordens. (`app/chamados/services/vigia_fio.py:266`; `app/chamados/tests/test_o261_vigia_fio.py:547–570`) |
   | `dias=_meus` no horário | **Sim:** chama `propostas()` e exige o argumento `dias`. (`app/escala/tests/test_o261_v_fase_na_lista.py:373–381`) | Só confere presença e mínimo; um conjunto errado com o mesmo mínimo sobrevive a esse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:379–381`) |
   | Seleção por assinatura no fio | **Sim, no auxiliar:** voltar à primeira quebra o teste com duas âncoras diferentes. (`app/escala/tests/test_o261_v_fase_na_lista.py:392–398`) | Não chama aplicar/recusar nem cobre A desaparecer e B sobreviver com assinatura igual. (`app/escala/tests/test_o261_v_fase_na_lista.py:394–398`) |
   | Recorte do chat | **Sim, no auxiliar:** retirar `colaborador_id` quebra a inspeção do argumento. (`app/escala/tests/test_o261_v_fase_na_lista.py:404–409`) | O retorno simulado está vazio; não exercita confirmação, vínculo escrito ou as outras APIs de frota. (`app/escala/tests/test_o261_v_fase_na_lista.py:407–409`) |

   Os testes anteriores percorrem aplicar/recusar com uma proposta e não conferem o vínculo entregue ao escritor no cenário de troca preservando assinatura. O chat também usa uma proposta e testa mudança de template ou desaparecimento total. Portanto, não fecham o residual A. (`app/chamados/tests/test_proposta_no_fio.py:112–122,170–178`; `app/core/tests/test_ia_escala_com_sim.py:57–59,86–118`)

## 30/10 19:22 · sessao 6f74f43b9a49 · - · - · origem=Ronald (tmux)

Os testes novos mordem algumas curas, mas não cobrem integralmente os consumidores: [nao verificado]

| Cura | O que o teste verifica | Lacuna | [nao verificado]
|---|---|---|
| Dono da lavra passa pelo ponto único | Executa `_lavrar()`, troca o helper por sentinela e exige essa sentinela em todos os itens gravados. Trocar a chamada por `'CADASTRO'` derruba o teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:341–345`; chamada real em `app/ponto/management/commands/censo_fase_12x36.py:262`) | Não decide a pergunta aberta de qual juiz responde o dono. (`app/escala/tests/test_o261_v_fase_na_lista.py:338–339`) |
| Ordem das assinaturas | Chama `dias_na_lista` com C9/BATIDA e C10/CADASTRO nas duas ordens. (`app/escala/tests/test_o261_v_fase_na_lista.py:319–324`) | Não leva esse conflito ao `vigia_fio`. O teste do consumidor fornece somente uma C9 por dia. (`app/chamados/tests/test_o261_vigia_fio.py:547–551`, `553–570`) |
| Horário recebe dias do vínculo | Executa `propostas()` e exige que `_horario_mediano` receba `dias`, com mínimo 20/10. Retirar `dias=_meus` derruba esse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:373–381`; `app/escala/services/propositor.py:529`) | A fixture possui um vínculo; conjunto errado que preserve o mínimo passa pelas asserções desse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:365–381`) |
| Fio recalcula pela assinatura | Duas propostas com âncoras diferentes; exige que `_recalcular` retorne o segundo vínculo. Voltar ao primeiro derruba esse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:392–398`) | Não chama `aplicar_do_fio` nem `recusar_do_fio`, e não cobre A desaparecer e B sobreviver com a mesma assinatura. (`app/escala/tests/test_o261_v_fase_na_lista.py:394–398`) |
| Chat estreita pela pessoa | Chama `_proposta_de_escala(7)` e exige `colaborador_id=7` no propositor. Retirar o keyword derruba esse teste. (`app/escala/tests/test_o261_v_fase_na_lista.py:404–409`) | Usa retorno sem propostas; não exercita seleção por assinatura nem aceite no request. (`app/escala/tests/test_o261_v_fase_na_lista.py:407–409`) |

Os testes antigos do fio exercitam aplicar/recusar com uma única proposta, sem conferir que o objeto enviado ao escritor é o segundo vínculo escolhido pela assinatura. (`app/chamados/tests/test_proposta_no_fio.py:112–122`, `170–178`)

Os testes antigos do chat passam pelo request, mas a fixture retorna uma proposta; o aceite confere colaborador, usuário e chamada. Os casos negativos mudam `template_id` ou esvaziam a lista, portanto não cobrem assinatura igual em vínculo diferente. (`app/core/tests/test_ia_escala_com_sim.py:57–59`, `86–118`)

Há uma inconsistência declarativa: a prosa chama o censo de juiz único, mas seu papel continua `lavra`; o corpo ainda contém “VIGIA, NUNCA JUIZ”. A lei específica citada no teste escolheu o censo como juiz; isso impede concluir apenas pela prosa que o predicado autorizado viola 4a. (`app/config/crons.py:600–605`, `2145–2151`; `app/ponto/management/commands/censo_fase_12x36.py:241–249`; `app/escala/tests/test_o261_v_fase_na_lista.py:4–8`)

O selo do tabuleiro verifica enumeração do papel e igualdade com a lista declarada; não verifica semanticamente se uma `lavra` julga. (`app/core/tests/test_contract_tabuleiro.py:15–28`)

Execução real dos testes, RED e mutações: `[nao verificado]`. [nao verificado]

## 30/10 19:22 · sessao 2599c17d63fd · - · - · origem=Ronald (tmux)

Achados para consolidação (só leitura; sem banco/log; ensaio executou corpos reais via AST em memória e dependências simuladas).

1. **A-residual permanece no fio e chat.** A assinatura ignora `vinculo_id` (`app/escala/services/propositor.py:663`); `da_assinatura` rejeita apenas quando encontra duas propostas simultâneas (`:677`, `:680`, `:684`). Contraexemplo: preview A/ec1; A some; sobra B/ec2 com mesmo conteúdo. `_recalcular` devolve B (`app/chamados/services/proposta_no_fio.py:109`), `aplicar_do_fio` escreve B (`:133`, `:146`) e `recusar_do_fio` recusa B (`:168`, `:171`). No chat, sessão guarda somente colaborador/assinatura (`app/core/views_ia.py:620`); recalcula e aplica B (`:649`, `:658`). O ensaio em memória confirmou ambos escritores recebendo ec2; ocorrência em produção `[nao verificado]`. A falha contradiz a proteção declarada pelo próprio gate contra aplicar sob texto de outra proposta (`app/chamados/services/proposta_no_fio.py:95`) e o contrato de ambiguidade (`app/escala/services/propositor.py:671`). A triagem anexada já descreve este residual, mas seu lastro externo é `[nao verificado]`.

2. **Frota ainda dentro de dois requests.**
   - Direto: rota `app/api/urls.py:55` → `api_mensageria_propostas_escala` → `propositor.propostas()` sem filtro (`app/api/views_mensageria.py:2005`, `:2019`).
   - Indireto: rota `app/api/urls.py:58` → revisão semanal (`app/api/views_mensageria.py:2189`, `:2205`, `:2207`) → `revisao_semanal_supervisao` (`app/escala/services/fase_12x36.py:174`) → `linhas_para_a_pauta` → `propostas(hoje)` (`app/escala/services/propositor.py:640`). Ausência de filtro escolhe toda frota (`:428`). Há caller mensageria (`mensageria/nucleo/core_client.py:287`, `:289`; `mensageria/nucleo/ferramentas.py:934`). Custo/tempo atual `[nao verificado]`; o contrato do propositor declara estreitamento para cliques (`app/escala/services/propositor.py:396`).

3. **Censo de decisões e leitores.**
   - `_recalcular` produz `(p, erro)` (`app/chamados/services/proposta_no_fio.py:92`, `:116`); consumidores são `aplicar_do_fio` (`:133`) e `recusar_do_fio` (`:168`), chamados externamente por ações do fio (`app/chamados/services/fio.py:505`, `:509`, `:511`).
   - `_proposta_de_escala` produz `(p, assinatura)` (`app/core/views_ia.py:562`, `:579`, `:582`); preview consome (`:591`) e guarda assinatura (`:620`); aceite consome (`:649`) e escreve pelo escritor de intake (`:658`). Despacho preview: `_avancar_intake` (`:461`, `:462`); despacho aceite: `ia_perguntar` (`:869`, `:872`) → `_aplicar_proposta` (`:147`, `:148`). Escritor/juiz/gate estão registrados em `app/core/intake.py:51`, `:52`, `:53`.
   - Preview do fio ainda seleciona primeiro (`app/chamados/services/proposta_no_fio.py:66`), lido nos dois renders (`app/chamados/services/fio.py:462`; `app/chamados/views.py:932`). Ficha também seleciona primeiro (`app/colaboradores/services/ficha.py:178`), mas só sugere ação (`:189`). Chat preview seleciona primeiro do colaborador (`app/core/views_ia.py:580`). Esses caminhos não são decisões diretas.
   - CLI escolhe `_dele[0]` somente após rejeitar `len>1` (`app/escala/management/commands/propostas_de_escala.py:31`, `:37`, `:38`), então não achei outro aplicador/recusador produtivo que selecione primeiro sem guarda no censo dos imports/calls encontrados. Resolução dinâmica fora do censo `[nao verificado]`.

4. **Censo produtivo de `propostas`:** fio por uma pessoa (`app/chamados/services/proposta_no_fio.py:63`, `:102`); chat por uma (`app/core/views_ia.py:575`); painel pelos IDs da página com `so_existencia` (`app/chamados/services/painel.py:518`, `:521`); ficha por uma (`app/colaboradores/services/ficha.py:176`); API frota e pauta frota acima; CLIs frota fora request (`app/escala/management/commands/propostas_de_escala.py:26`; `app/escala/management/commands/ritmo_discordante.py:18`).

5. **Os testes novos mordem helpers, parcialmente o chamador.** [nao verificado]
   - Teste do fio chamado “proposta aplicada” chama apenas `_recalcular`, com duas assinaturas diferentes (`app/escala/tests/test_o261_v_fase_na_lista.py:387`, `:394`, `:396`, `:398`); não verifica o objeto entregue a `aplicar_do_fio`/`recusar_do_fio` e não contempla A-some/B-sobra. Reverter escolha por assinatura para primeira quebra esse teste; bypass nos chamadores pode sobreviver a ele.
   - Teste de duas assinaturas iguais testa somente `da_assinatura` (`:263`, `:272`), não seus consumidores. [nao verificado]
   - Teste de estreitamento chama `_proposta_de_escala` e inspeciona argumento de `propostas` (`:404`, `:408`, `:409`); retirar `colaborador_id` quebra o teste, mas não mede custo nem percorre os dois requests de frota. [nao verificado]
   - Testes antigos do fio exercitam portas com proposta única (`app/chamados/tests/test_proposta_no_fio.py:112`, `:118`, `:170`, `:175`); os de chat usam proposta única (`app/core/tests/test_ia_escala_com_sim.py:57`, `:59`, `:86`). Não cobrem troca de vínculo preservando assinatura.

Referência das leis: `L-041` (`app/docs/LEIS.md:89`), `L-042` (`:90`), `L-099` (`:122`); separação consumidor/lâmpada/cron (`CLAUDE.md:273`, `:275`, `:276`). `JUIZES.md` distingue geometria de turno (`app/docs/JUIZES.md:20`), dia de trabalho (`:66`), vínculo do dia (`:75`) e cobertura da data (`:77`), e explicita que é lookup, com registro vivo em `core/juizes.py` (`:7`, `:10`).

_743 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
