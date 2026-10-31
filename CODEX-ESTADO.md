# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 16:16** · publicado em **31/10/2026 16:17** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 16:16 · sessao fb824fd80c4f · ADR-0003 · censo · origem=Code

Leitura estática do código. **O total absoluto de leitores do repositório ficou `[nao verificado]`**: abaixo conto funções comprovadas, com até oito exemplos por campo. `≥8` significa oito leitores comprovados apresentados; `0 localizado` não prova inexistência. Execução em produção, conteúdo efetivamente armazenado no JSON e correspondência do snapshot com o HASH informado: `[nao verificado]`.

Os caminhos são relativos a `app/`. Para reduzir repetição nas tabelas, as referências abreviadas têm estas expansões literais:

| Referência | Arquivo |
|---|---|
| `M:linha` | `escala/models.py:linha` |
| `P:linha` | `ponto/portas/celula.py:linha` |
| `C:linha` | `ponto/services/cartorio.py:linha` |
| `G:linha` | `ponto/management/commands/gerar_celulas.py:linha` |
| `L:linha` | `escala/services/leitor_celula.py:linha` |
| `J:linha` | `chamados/juizes.py:linha` |
| `F:linha` | `folha/export.py:linha` |
| `S:linha` | `ponto/status_dia.py:linha` |
| `T:linha` | `ponto/turnos.py:linha` |
| `U:linha` | `escala/utils.py:linha` |

Os nomes abreviados de escritores usados abaixo são:

| Nome | Função e persistência |
|---|---|
| **GER** | `gerar_celulas_periodo`: cria e salva células — `P:593`, `P:605`, `P:615`. |
| **REG** | `regenerar_celulas_vinculo`: cria, limpa previsão fora da vigência ou regrava previsão — `P:231`, `P:247`, `P:259`. |
| **REV** | `reverter_regeneracao_dia`: restaura DNA anterior e salva — `P:527`, `P:538`. |
| **LAV** | `lavrar_veredito`: atribui julgamento/status e persiste por `salvar_e_emitir` — `P:709`, `P:743`, `P:760`, `P:775`. |
| **CAR** | `carimbar`: grava impressão, datas e insumos, incluindo campos preparados por LAV — `P:787`, `P:800`, `P:807`. |
| **PREC** | `carimbar_precedencia`: grava `ata.precedencia` — `P:811`, `P:819`, `P:822`. |
| **LOTE** | `carimbar_insumos_em_lote`: atribui insumos e executa `bulk_update` — `P:826`, `P:838`, `P:843`. |
| **SAVE** | `salvar_e_emitir`: `cel.save(update_fields=campos)`, compartilhado por LAV/CAR/PREC — `P:634`, `P:640`. |

`regenerar_celulas_dia` delega a REG com janela de um dia; não contém outra gravação (`P:474`). A fachada `CelulaDia.lavrar_veredito` delega a LAV (`M:1794`).

Os portões e eventos referenciados nas tabelas significam:

| Código | Fato comprovado |
|---|---|
| **PG** | GER pula `origem='editada'`; no passado só preenche DNA nulo; hoje/futuro podem ser regravados. Nenhuma consulta de exportação, tranca, `FechamentoMensal` ou versão nesse corpo — `P:555`, `P:598`, `P:600`, `P:609`. |
| **PR** | REG e REV consultam `_competencias_exportadas`; `apesar_da_lavra` dispensa essa guarda — `P:196`, `P:199`, `P:238`, `P:520`. A consulta usa `competencia_fechada_para_celula`, que combina TXT vigente com holerite do colaborador em lote publicado — `G:173`, `F:599`, `F:603`. `FechamentoMensal.status` não é esse portão — `G:132`. REG também protege origem humana — `P:226`, `P:251`; REV exige origem gerada e DNA anterior — `P:518`. | [nao verificado]
| **PJ** | Lavra/carimbo normais: **nenhum portão de competência exportada/trancada ou `FechamentoMensal`** nos escritores — `P:709`, `P:787`, `P:811`. Há idempotência por conteúdo — `P:753`, `P:795`, `P:816`. O cartório compara impressão; `forcar` permite rejulgamento — `C:1157`, `C:1161`. |
| **PS** | Exclusivamente no command `lavrar_status_competencia`: barra `competencia_entregue` — `ponto/management/commands/lavrar_status_competencia.py:40`; completa somente status nulo com carimbo atual — `C:726`, `C:729`, `C:733`. |
| **PB** | Backfill de insumos: nenhum portão de competência; seleciona células julgadas e chama LOTE — `ponto/management/commands/backfill_b5_insumos.py:60`, `:72`, `:81`. |
| **EG** | GER pelo cron 05:50 — `config/crons.py:407`; alterações de vigência/âncora chamam REG — `escala/signals.py:71`, `:117`; alteração de template — `escala/services/cadastro_tipo.py:348`; atualização de folgas — `escala/folgas.py:220`; edição do vínculo — `colaboradores/services/vinculo.py:320`. |
| **EJ** | Cartório pelo cron 06:27 — `config/crons.py:597`; vigia de hora a cada dez minutos — `config/crons.py:348`; signals de batida save/delete, ausência save/delete e justificativa save — `ponto/signals.py:132`, `:139`, `:144`, `:151`, `:162`; chamado criado/mudança de status — `chamados/signals.py:165`, `:180`. |
| **EA** | Decisão humana chama `cel.lavrar_veredito(via='admin', …)` — `ponto/services/veredito_celula.py:330`. |
| **EB** | Backfill de insumos — `ponto/management/commands/backfill_b5_insumos.py:72`, `:81`; lote de status — `ponto/management/commands/lavrar_status_competencia.py:55`. |

Na coluna **FP**, “não” significa que **o valor da coluna não é serializado diretamente no fingerprint**. A definição integral das partes está em `C:333`–`C:390`; influências indiretas por seleção de insumos aparecem depois no grafo.

| Coluna | Escritores de produção | Fonte do valor gravado | Regravação/evento | Portão | Leitores comprovados; contagem | FP |
|---|---|---|---|---|---|---|
| `colaborador` | GER `P:593`; REG `P:247`. | `ec.colaborador_id` — `P:247`, `P:594`. | Só criação nos escritores encontrados; alteração posterior `[nao verificado]`. | GER: PG; REG: PR. | **≥8**: `celulas_do_periodo` (`escala/alimentacao.py:42`); `escala_vigente` (`escala/servico_jornada.py:46`); `propostas` (`escala/services/propositor.py:488`); `montar_previa` (`folha/previa.py:51`); `dia_da_frota` (`relatorios/dia_da_frota.py:42`); `lastro.julgar` (`ponto/services/lastro.py:233`); `aplicar_precedencia_na_celula` (`C:1674`); `emitir_evento_celula` (`P:631`). | Não — `C:333`. |
| `data` | GER `P:594`; REG `P:247`. | Dia iterado na janela solicitada — `P:208`, `P:571`. | Só criação nos escritores encontrados; alteração posterior `[nao verificado]`. | GER: PG; REG: PR. | **≥8**: `celulas_do_periodo` (`escala/alimentacao.py:42`); `escala_vigente` (`escala/servico_jornada.py:46`); `propostas` (`escala/services/propositor.py:488`); `grade_da_celula` (`L:325`); `motivos_retencao_celula` (`F:1188`); `montar_previa` (`folha/previa.py:77`); `orfas_e_isencao_da_ata` (`relatorios/cartao_pela_celula.py:288`); `emitir_evento_celula` (`P:631`). | Não diretamente; seleciona janela/versão — `C:1104`, `C:1148`, `C:947`. |
| `trabalha` | GER `P:595`, `P:612`; REG `P:231`, `P:247`, `P:260`; REV `P:532`. | Cálculo EC/template — `P:243`, `P:590`; FolgaDia — `M:1257`, `M:1327`; âncora/ciclo — `M:1260`; feriado/praça — `M:1340`, `M:1362`; REV usa `dna_anterior.marcos` — `P:527`, `P:532`. | Sim: EG, regeneração explícita e reversão — `P:260`, `P:532`. | PG/PR conforme escritor. | **≥8**: `dia_sem_trabalho` (`J:408`); `eh_dia_trabalho` (`M:1072`); `grade_da_celula` (`L:345`); `classificar_falta` (`escala/servico_jornada.py:303`); `contexto_do_dia_texto` (`pautas/services.py:410`); `etiquetas_pela_celula` (`ponto/services/flip_auto.py:208`); `_veredito_do_dia` (`ponto/services/dia_do_colab.py:121`); `dia_da_frota` (`relatorios/dia_da_frota.py:47`). | Não — `C:333`. |
| `origem` | GER/REG gravam `'gerada'` — `P:248`, `P:595`; default — `M:1659`. | Literal/default, não batidas — mesmas linhas. | Escritor posterior de `'editada'` não localizado `[nao verificado]`. | Criação GER: nenhum de competência; REG: PR. | **6**: `CelulaDia.__str__` (`M:1810`); `celulas_contra_o_template.Command.handle` (`escala/management/commands/celulas_contra_o_template.py:74`); GER (`P:598`); REG (`P:226`); REV (`P:518`); `dia_do_colab` (`ponto/services/dia_do_colab.py:90`). | Não — `C:333`. |
| `escala_geradora` | GER `P:595`, `P:613`; REG `P:248`, `P:260`; REV `P:535`; declaração `SET_NULL` — `M:1661`. | EC fornecida; na reversão, `dna_anterior.vinculo_id` — `P:533`. | Sim: EG/REG/REV; deleção da EC pode nulificar pela relação — `M:1661`. | PG/PR; `SET_NULL`: nenhum portão de competência declarado no campo — `M:1661`. | **7**: `vinculo_do_dia` (`escala/alimentacao.py:238`); `contraprova_piso` (`chamados/management/commands/auditar_invariantes_chamados.py:389`); GER (`P:611`); REG (`P:228`); `regenerar_em_exportada` (`ponto/portas/regen_exportada.py:141`); `turnos_abertos_de` (`T:2316`); `dia_da_frota`, pré-carga utilizada na consulta do vínculo (`relatorios/dia_da_frota.py:35`, `:42`). | Não — `C:333`. |
| `dna` | GER `P:596`, `P:604`, `P:614`; REG `P:232`, `P:248`, `P:260`; REV `P:530`. | `montar_dna`: template/marcos/pausas/ciclo/jornada/intervalo/extra declarada — `G:76`, `G:81`, `G:95`, `G:119`; condicionado a `trabalha` — `G:79`; REV copia `dna_anterior` — `P:527`. | Sim: EG/REG/REV; GER também enriquece DNA nulo passado — `P:603`. | PG/PR. | **≥8**: `marcos_do_dia` (`M:1099`); `pausas_do_dia` (`M:1162`); `extra_declarada_do_dia` (`M:1206`); `marcos_dna_periodo` (`U:97`); `pausas_de_celulas` (`U:130`); `contexto_do_dia_texto` (`pautas/services.py:406`); `periodos_do_dia` (`ponto/juiz_batida.py:158`); `_julgar_colab_corpo` (`C:998`). | **Sim: somente `dna_versao` e `marcos`** — `C:337`. |
| `gerada_em` | `auto_now_add`, nos creates GER/REG — `M:1670`, `P:247`, `P:593`. | Relógio na criação — `M:1670`. | Não regravado pelos caminhos encontrados; comportamento declarado — `M:1675`. | Criação GER/REG conforme PG/PR. | **2**: `marcos_do_lote` (`chamados/veredito_lavrado.py:153`); `janelas_reabertas` (`escala/services/fase_12x36.py:190`). | Não — `C:333`. |
| `regenerada_em` | REG `P:262`; REV `P:536`. | `timezone.now()` — mesmas linhas. | Sim em regeneração alterada/reversão. O ramo de limpeza fora da vigência salva só `trabalha/dna` — `P:231`, `P:233`. | PR. | **7**: `marcos_do_lote` (`chamados/veredito_lavrado.py:153`); `janelas_reabertas` (`escala/services/fase_12x36.py:189`); `houve_fato_novo` (`C:295`); `dia_do_colab` (`ponto/services/dia_do_colab.py:91`); `fila_de_orfas` (`escala/services/regeneracao.py:145`); `_marcar_cobrancas_orfas` (`G:242`); `supra_juiz.Command.handle` (`ponto/management/commands/supra_juiz.py:153`). | Não — `C:333`. |
| `regeneracoes` | Default `0` — `M:1691`; REG `P:263`; REV `P:537`. | Valor anterior, ou zero, mais um — `P:263`, `P:537`. | Sim em REG/REV; GER não grava contador — `P:615`; limpeza fora da vigência também não — `P:233`. | PR. | **2**: REG (`P:263`); REV (`P:537`). | Não — `C:333`. |
| `dna_anterior` | REG `P:261`; REV limpa — `P:531`. | DNA imediatamente anterior — `P:259`; na reversão, literal `None` — `P:531`. | Sim: sobrescrito por nova regeneração; apagado na reversão — mesmas linhas. | PR. | **2**: REV (`P:518`, `P:527`); `aplicar_col900.Command.handle` (`ponto/management/commands/aplicar_col900.py:68`). | Não — `C:333`. |
| `editada_em` | Nenhum escritor de produção localizado `[nao verificado]`; o serviço registra “nenhum declarado hoje” — `escala/services/fase_12x36.py:161`. | `[nao verificado]`. | `[nao verificado]`. | `[nao verificado]`. | **3**: `marcos_do_lote` (`chamados/veredito_lavrado.py:154`); `janelas_reabertas` (`escala/services/fase_12x36.py:190`); `houve_fato_novo` (`C:295`). | Não — `C:333`. |
| `editada_por` | Nenhum escritor explícito localizado `[nao verificado]`; relação declara `SET_NULL` — `M:1701`. | Escrita humana `[nao verificado]`; nulificação por deleção declarada — `M:1702`. | Deleção do usuário: `SET_NULL` — `M:1702`; demais eventos `[nao verificado]`. | Nenhum no campo — `M:1702`; demais caminhos `[nao verificado]`. | **0 localizado** `[nao verificado]`. | Não — `C:333`. |
| `julgada_em` | CAR/SAVE — `P:800`, `P:807`, `P:640`. | Argumento `agora` do julgamento — `P:800`. | Sim por EJ quando impressão/insumos/lavra diferem; repetição idêntica retorna sem salvar — `P:795`. | PJ. | **7**: `marcos_do_lote` (`chamados/veredito_lavrado.py:154`); `janelas_reabertas` (`escala/services/fase_12x36.py:189`); CAR (`P:796`); `_julgar_colab_corpo` (`C:1000`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:109`); `backfill_b5_insumos.Command.handle` (`ponto/management/commands/backfill_b5_insumos.py:61`); `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:55`). | Não — `C:333`. |
| `impressao` | CAR/SAVE — `P:801`, `P:807`. | `impressao_insumos` — `C:998`, `C:1148`; composição detalhada abaixo — `C:333`. | Sim por EJ; comparação controla pulo — `C:1157`; `forcar` ignora pulo — `C:1161`. | PJ; versão participa do hash, não é bloqueio — `C:352`. | **3**: CAR (`P:797`); `_julgar_colab_corpo` (`C:1000`, `C:1157`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:110`). | Não é insumo do próprio hash — `C:333`. |
| `veredito` | LAV, persistido por SAVE/CAR — `P:770`, `P:775`, `P:807`. | Cartório: literal ou `derivar_veredito(codigos)` — `P:658`; códigos vêm da grade, batidas, cobertura, chamados/fio e cadastro — `C:1207`; humano fornece decisão — `P:750`, `ponto/services/veredito_celula.py:330`. | Sim por EJ/EA; cartório preserva decisão admin — `P:738`; idempotência — `P:753`. | PJ; precedência do admin é guarda de decisão, não de competência — `P:738`. | **≥8**: `_trilha_da_recusa` (`chamados/models.py:1148`); `retratar_chamados_absolvidos_por_celula` (`chamados/reconciliador.py:963`); `_absolvido_pela_celula` (`chamados/reconciliador.py:982`); `motivos_a_perguntar` (`chamados/services/fabrica_da_celula.py:86`); `grade_da_celula` (`L:365`); `motivos_retencao_celula` (`F:1215`); `montar_previa` (`folha/previa.py:94`); `dia_da_frota` (`relatorios/dia_da_frota.py:51`). | Não — `C:333`. |
| `veredito_via` | LAV — `P:771`, `P:775`. | Argumento validado: `cartorio/admin` — `P:720`, `P:771`. | Sim por EJ/EA, respeitando decisão admin — `P:738`. | PJ. | **≥8**: `veredito_que_a_lavra_escreve` (`P:656`); `_status_alvo` (`P:694`); LAV (`P:722`); `_trilha_da_recusa` (`chamados/models.py:1154`); `retratar_chamados_absolvidos_por_celula` (`chamados/reconciliador.py:964`); `_chave_memo_pv` (`J:793`); `_pergunta_viva_calc` (`J:1046`); `contexto_calendario` (`colaboradores/services/calendario.py:513`). | Não — `C:333`. |
| `veredito_em` | LAV — `P:772`, `P:775`. | `agora or timezone.now()` — `P:772`. | Sim quando muda julgamento/ata; completar só status não move essa data — `P:753`, `P:757`. | PJ; lote de status PS não move o campo — `P:757`. | **4**: `marcos_do_lote` (`chamados/veredito_lavrado.py:153`); `contexto_calendario` (`colaboradores/services/calendario.py:513`); `decisao_do_dia` (`ponto/services/veredito_celula.py:109`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:107`). | Não — `C:333`. |
| `protestos` | LAV — `P:769`, `P:775`. | Cartório copia códigos atuais — `P:746`, `C:1207`; admin preserva a própria coluna — `P:751`. | Sim por EJ; EA pode salvar a lista preservada — `P:751`, `P:769`. | PJ. | **6**: `motivos_retencao_celula` (`F:1228`); `nao_retem_pela_pausa` (`F:1147`); LAV (`P:722`); `_completar_status` (`C:733`); `julgar_colab` (`C:864`); `celula_veredito_velho.Command.handle` (`ponto/management/commands/celula_veredito_velho.py:108`). | Não — `C:333`. |
| `ata` | LAV `P:741`, `P:768`; PREC `P:819`; persistência SAVE/CAR — `P:640`, `P:807`. | Grade/DNA/batidas/ausências/relógio — `C:564`, `C:610`, `C:713`; precedência — `C:1674`. LAV preserva `precedencia` anterior — `P:732`. | Sim por EJ; precedência aplicada no julgamento unitário após a lavra — `C:1721`; mesmo veredito com ata diferente também relavra — `P:735`, `P:767`. | PJ; decisão admin não impede atualizar ata — `P:738`, `P:741`. | **≥8**: `dia_sem_trabalho` (`J:407`); `_celula_furada` (`J:442`); `grade_da_celula` (`L:331`); `montar_previa` (`folha/previa.py:83`); `referencia_do_espelho` (`folha/validacao_pdf.py:47`); `periodos_do_dia` (`ponto/juiz_batida.py:157`); LAV (`P:731`); PREC (`P:816`). | Não diretamente — `C:333`; pode influenciar insumos indiretos, conforme grafo. |
| `ausencia` | CAR `P:802`; LOTE `P:838`, `P:843`; relação `SET_NULL` — `M:1764`. | `ausencia_cobre(..., para='cobranca')` — `C:981`, `C:1106`; backfill — `ponto/management/commands/backfill_b5_insumos.py:72`. | Sim por EJ/EB; deleção também pode nulificar — `M:1765`. | CAR: PJ; LOTE: PB; `SET_NULL`: nenhum declarado no campo — `M:1765`. | **4**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:75`); CAR (`P:797`); LOTE (`P:835`); `assinaturas_do_colab` (`ponto/services/esmeril_espelho.py:133`). | Não: o hash recebe **status da cobertura consultada**, não esta FK — `C:335`, `C:1106`, `C:1148`. |
| `tem_entrada` | CAR `P:803`; LOTE `P:839`, `P:843`. | `insumos_da_ata(ata,dna)`: lâmpada externa E/`dna.marcos.hi`; sem lâmpadas consulta `n_orfas` — `C:643`, `C:649`, `C:650`, `C:665`. | Sim por EJ/EB — `C:1578`, `ponto/management/commands/backfill_b5_insumos.py:73`. | PJ/PB. | **3**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:80`); CAR (`P:798`); LOTE (`P:836`). | Não — `C:333`. |
| `tem_saida` | CAR `P:803`; LOTE `P:839`, `P:843`. | Mesma função, lâmpada externa S/`dna.marcos.hf` — `C:650`, `C:655`, `C:665`. | Sim por EJ/EB — `C:1578`, `ponto/management/commands/backfill_b5_insumos.py:73`. | PJ/PB. | **3**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:80`); CAR (`P:798`); LOTE (`P:836`). | Não — `C:333`. |
| `insumos_em` | CAR `P:804`; LOTE `P:840`, `P:843`. | Argumento `agora`; backfill lê relógio — `P:804`, `ponto/management/commands/backfill_b5_insumos.py:59`. | Sim quando carimbo/insumos diferem; repetição idêntica não grava — `P:795`, `P:835`. | PJ/PB. | **3**: `vigia_b5_ausencia.medir` (`ponto/management/commands/vigia_b5_ausencia.py:71`); CAR (`P:796`); LOTE (`P:835`). | Não — `C:333`. |
| `status_dia` | `_aplicar_status`, dentro de LAV; SAVE/CAR persistem — `P:705`, `P:743`, `P:760`, `P:773`. | `_status_alvo`: veredito/via + fatos atuais da grade — `P:694`, `P:698`; mapa admin — `S:52`, `S:64`; fatos incluem batidas, previsto/realizado, isenção, parcial e precedência humana — `S:124`, `S:143`. | Sim por EJ/EA; EB completa somente nulo/carimbo atual — `C:726`, `C:733`. | Lavra normal PJ; command PS. | **5**: `status_do_dia_lavrado` (`S:181`); `_completar_status` (`C:726`); `_status_alvo` (`P:697`); `_aplicar_status` (`P:703`); LAV (`P:723`). | Não — `C:333`. |
| `status_rotulo` | Mesmo escritor/persistência de status — `P:705`, `P:743`, `P:760`, `P:773`. | Regra de status e vocabulário; exceção de rótulo do isento — `S:28`, `S:48`, `S:60`, `S:120`. | Sim por EJ/EA/EB, no mesmo ato do status — `P:705`, `C:733`. | PJ/PS. | **3**: `status_do_dia_lavrado` (`S:182`); `_status_alvo` (`P:697`); `_aplicar_status` (`P:703`). | Não — `C:333`. |

O produtor atual emite nove chaves de topo em `ata_do_dia`; PREC acrescenta a décima, `precedencia` (`C:567`, `C:591`, `P:819`). Há oito chaves possíveis em cada item de `lampadas`, contando as opcionais (`C:501`, `C:540`, `C:424`, `C:620`, `C:714`), e seis dentro de `precedencia` (`C:1675`). Os elementos de `orfas` e `precedencia.suprimidos` são strings, sem subchaves (`C:574`, `ponto/precedencia.py:351`).

Na tabela seguinte, **LAV/CAR** significa a preparação de `ata` por LAV e sua persistência por SAVE ou CAR (`P:768`, `P:775`, `P:807`). **EJ/PJ** mantém exatamente os eventos e portões definidos acima. Todas as chaves têm **FP não diretamente**, pelo corpo de `impressao_insumos` (`C:333`–`C:390`).

| Chave de `ata` | Escritores de produção | Fonte do valor gravado | Regravação/evento | Portão | Leitores comprovados; contagem | FP |
|---|---|---|---|---|---|---|
| `tipo_dia` | `ata_do_dia` → LAV/CAR — `C:567`. | `dia.tipo_dia`; overlays FolgaDia/Ausencia/isenção na grade — `U:1109`, `U:1137`, `U:1785`. | Sim, EJ — `C:1557`. | PJ. | **3**: `dia_sem_trabalho` (`J:408`); `grade_da_celula` (`L:344`); `medir_ata_fantasma` (`ponto/management/commands/tripwire_celulas.py:342`). | Não — `C:333`. |
| `lampadas` | `lampadas_do_dia`/`ata_do_dia` → LAV/CAR — `C:565`; transformações por dispensa/teto — `C:615`, `C:712`. | Match da grade, marcos/DNA, órfãs e sinal de realizado — `C:444`, `C:456`, `C:565`. | Sim, EJ — `C:1557`. | PJ. | **≥8**: `_celulas_da_ata` (`L:93`); `n_ok_lavrado` (`L:81`); `_celula_furada

[truncado em 20000 de 37797 caracteres]

## 31/10 16:12 · sessao 16cb111707cb · - · - · origem=Ronald (tmux)

Leitores enviados nas mensagens anteriores, com até oito funções comprovadas por coluna/chave e contagens do conjunto enumerado. O total absoluto do repositório permanece `[nao verificado]`.

Correções finais:

- `ata.precedencia`: **2 leitores confirmados** — `carimbar_precedencia` (`app/ponto/portas/celula.py:816`) e `lavrar_veredito`, que preserva a chave durante a substituição da ata (`app/ponto/portas/celula.py:732`).
- `julgada_em`: **7 funções confirmadas** nas referências enviadas; `processar_cartorio` não conta como leitor direto desse campo: seleciona por colaborador/data e delega (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:61`).
- `regenerada_em`: retire a referência a `regenerar_celulas_vinculo:207`; aquela linha lê `data`, não `regenerada_em` (`app/ponto/portas/celula.py:207`).
- Os nomes corretos são `_chave_memo_pv` para a leitura de veredito/via (`app/chamados/juizes.py:793`) e `decisao_do_dia` para a leitura ORM de veredito/via/em (`app/ponto/services/veredito_celula.py:109`).
- `ata.marcos`: nenhuma chave produtora/leitor direto localizado `[nao verificado]`; o construtor atual emite `lampadas`, não `marcos` (`app/ponto/services/cartorio.py:567`).
- `editada_por`, `ata.lampadas[].dispensada` e os seis filhos de `ata.precedencia`: nenhum leitor individual localizado; exaustividade `[nao verificado]`.
- Os totais enviados cobrem funções confirmadas de `app/`, incluindo comandos operacionais; total semântico dos scripts de `bin/` `[nao verificado]`.

## 31/10 16:12 · sessao 758e4f350d15 · - · - · origem=Ronald (tmux)

Todos os caminhos abaixo são leitura estática; referências relativas à raiz.

| Campo | Escrita, fonte e regravação comprovadas |
|---|---|
| `julgada_em` | `carimbar`: recebe `agora`, grava `cel.julgada_em=agora`; salva por `salvar_e_emitir` (`app/ponto/portas/celula.py:800`, `:807`, `:640`). Regrava quando hash/ausência/pontas/lavra diferem; caso idêntico retorna sem salvar (`:795-799`). |
| `impressao` | `carimbar`, valor `impressao_insumos`; atribuição `app/ponto/portas/celula.py:801`; chamadas do cartório `app/ponto/services/cartorio.py:998`, `:1148`, `:1579`. |
| `ausencia` | `carimbar` e `carimbar_insumos_em_lote`, atribuições `app/ponto/portas/celula.py:802`, `:838`; `bulk_update` `:843`. Fonte `ausencia_cobre(...para='cobranca')`: `app/ponto/services/cartorio.py:981`, `:1106`; backfill `app/ponto/management/commands/backfill_b5_insumos.py:72`. |
| `tem_entrada`, `tem_saida` | `carimbar` e lote: `app/ponto/portas/celula.py:803`, `:839`, `:843`. Fonte `insumos_da_ata(ata,dna)` `app/ponto/services/cartorio.py:1578`; lê `ata.lampadas` e `dna.marcos.hi/hf`, confrontando tipo/hora/acesa; sem lâmpadas usa `n_orfas` (`:643-665`). |
| `insumos_em` | `carimbar` e lote; relógio `agora` (`app/ponto/portas/celula.py:804`, `:840`). |
| `veredito` | `lavrar_veredito` atribui/salva (`app/ponto/portas/celula.py:770`, `:775`). Cartório usa literal ou `derivar_veredito(codigos)` (`:647-664`); fonte normal classificação da grade, cobertura, batidas, chamados/fio, cadastro e relógio (`app/ponto/services/cartorio.py:1204-1217`, `:1244-1245`). Admin prevalece sobre cartório (`app/ponto/portas/celula.py:738-744`). |
| `veredito_via` | `lavrar_veredito`, parâmetro `via` (`app/ponto/portas/celula.py:771`, `:775`); validado contra vias declaradas (`:720-721`). |
| `veredito_em` | `lavrar_veredito`, `agora or timezone.now()` (`app/ponto/portas/celula.py:772`). Atualização exclusiva do status não o move (`:753-766`). |
| `protestos` | `lavrar_veredito`, cartório copia `codigos`, admin preserva lista existente (`app/ponto/portas/celula.py:746`, `:751`, `:769`). |
| `ata` | `lavrar_veredito` (`app/ponto/portas/celula.py:741`, `:768`, `:775`) e `carimbar_precedencia` (`:819-822`). Lavra preserva somente chave de outra operação `precedencia` (`:94`, `:730-734`). |
| `status_dia`, `status_rotulo` | `_aplicar_status` atribui ambos (`app/ponto/portas/celula.py:701-706`), persistidos por `lavrar_veredito` (`:743`, `:760`, `:773-775`). Fonte `_status_alvo`: decisão admin ou `status_da_lavra(veredito=v, **fatos)` (`:686-698`). Fatos da grade: tipo, previsto, batidas casadas/órfãs/realizado, isenção e parcial; para `indefinida`, precedência de FolgaDia/feriado (`app/ponto/status_dia.py:124-149`). Regras/rotulagem `:60-121`. |

Portões e gatilhos:

- Cartório normal: fila de **todas** as células passadas; `processar_cartorio.Command.handle` não recorta exportação/tranca/FechamentoMensal (`app/ponto/management/commands/processar_cartorio.py:49-65`). Funções de persistência acima tampouco consultam esses portões (`app/ponto/portas/celula.py:709-843`).
- `REGRA_VERSOES=((1,'2026-09'),(2,'2026-10'))` (`app/ponto/services/cartorio.py:110`). `versao_para` usa `janela_atual` e maior versão cujo horizonte alcança competência (`:191-197`): entra no hash (`:352-353`), não impede escrita. `forcar` ignora igualdade da impressão (`:1161-1163`).
- `lavrar_status_competencia` **barra entregue** por `competencia_entregue` (`app/ponto/management/commands/lavrar_status_competencia.py:40-42`). Executa `julgar_colab(completar_status=True)` (`:55-56`); só completa status NULL com carimbo atual, lendo `veredito/protestos` existentes (`app/ponto/services/cartorio.py:726-735`). Não confundir essa guarda com a lavra cotidiana.
- Backfill insumos seleciona apenas `julgada_em NOT NULL`, não tem corte competência; lê ausência/ata/dna e chama lote (`app/ponto/management/commands/backfill_b5_insumos.py:60-84`).
- Cron real `processar_cartorio --apply`: **06:27**, dependente de `reconciliar_grade` (`app/config/crons.py:597`); comentários do serviço citando 06:28 não são o agendamento atual. Vigia de hora: `*/10 * * * *`, `--apply` (`:348`).
- Signals: batida save/delete (`app/ponto/signals.py:132-140`); Ausencia save/delete (`:144-152`); Justificativa save (`:162-165`); todos chegam a `julgar_celula` (`:85-93`). Chamado criado ou com mudança `status_local`: `app/chamados/signals.py:165-180`.
- `julgar_celula` barra futuro/inexistente e, salvo teto, espera turno de hoje (`app/ponto/services/cartorio.py:1700-1713`); aplica precedência após julgar (`:1721-1723`).

`ata` atual tem **10 chaves de topo**: nove produzidas por `ata_do_dia`, mais `precedencia`. `ata.marcos` não aparece nos produtores atuais abaixo; `marcos` pertence a `dna.marcos` ou à grade (`app/ponto/services/cartorio.py:444-449`, `:932`).

| Chave de ata | Produtor/fonte |
|---|---|
| `tipo_dia` | `ata_do_dia`: `dia.tipo_dia` (`app/ponto/services/cartorio.py:567`). |
| `lampadas` | `ata_do_dia` chama `lampadas_do_dia` com células da grade, marcos, DNA, órfãs e sinal de realizado (`:564-567`). |
| `minutos_previstos` | Inteiro de `dia.minutos_previstos` (`:568`); grade calcula segmentos previstos E→S (`app/escala/utils.py:1578-1582`); intermitente pode copiar realizado (`:2008-2010`). |
| `minutos_realizados` | Inteiro de `dia.minutos_realizados` (`app/ponto/services/cartorio.py:569`); grade usa `realizado_do_dia` e zero explícito quando sem turno (`app/escala/utils.py:1985-1998`). |
| `sem_ancora` | Booleano do dia da grade (`app/ponto/services/cartorio.py:570`). |
| `n_orfas` | `len(dia.orfas)` (`:571`). |
| `orfas[]` | Horas convertidas em `str`; elementos sem subchaves (`:574`). |
| `n_missing` | Contagem `lampadas[].acesa is False`, ou missing da grade (`:575-576`); recalculada após dispensa (`:622`) e teto (`:716`). |
| `n_celulas` | `len(lampadas)` (`:591`). |
| `lampadas[].tipo` | Papel do marco do DNA/grade (`:449`, `:501`, `:540`, `:551`). |
| `lampadas[].hora` | Hora prevista do DNA/grade (`:501`, `:540`, `:551`). |
| `lampadas[].acesa` | Match da grade, proximidade quando marco ausente da régua, ou desconhecido (`:495-510`, `:540-553`); ausência aprovada integral/justificativa pode tornar True (`:610-620`); teto torna None (`:713-714`). |
| `lampadas[].luz` | Hora real casada, ou None (`:501-507`, `:541`, `:552`); dispensa zera luz (`:619`). |
| `lampadas[].tipo_real` | Tipo real fornecido pela grade; None quando ausente/proximidade sem tipo (`:503-507`, `:542`, `:553`). |
| `lampadas[].isencao` | Opcional True quando célula da grade marca isenção (`:424-425`, `:543`, `:554`). |
| `lampadas[].dispensada` | Opcional tipo da Ausencia integral aprovada, ou `'justificativa'` (`:610-620`). |
| `lampadas[].pendente` | Opcional True quando marco ainda não venceu (`:713-714`). |
| `precedencia` | `aplicar_precedencia_na_celula` consulta `fatos_do_dia` e chama `carimbar_precedencia` (`:1673-1682`); grava `app/ponto/portas/celula.py:819-822`. |
| `precedencia.tipo_do_dia` | Vencedor dos fatos (`app/ponto/services/cartorio.py:1676`; `app/ponto/precedencia.py:349-356`). |
| `precedencia.faixa` | Faixa do vencedor (`app/ponto/services/cartorio.py:1677`; `app/ponto/precedencia.py:350`, `:357`). |
| `precedencia.rotulo` | `ROTULOS`/tipo capitalizado (`app/ponto/services/cartorio.py:1678`; `app/ponto/precedencia.py:358`). |
| `precedencia.suprimidos[]` | Lista de **strings**, faixas perdedoras únicas (`app/ponto/services/cartorio.py:1679`; `app/ponto/precedencia.py:351-354`). Não contém dicionários/subchaves; vocabulário `PRECEDENCIA` (`:46`). |
| `precedencia.era_dia_previsto` | `trabalha` da célula, ou cálculo EC quando desconhecido (`app/ponto/services/cartorio.py:1680`; `app/ponto/precedencia.py:305-327`). |
| `precedencia.texto` | `texto_da_supressao(f)`; deriva vencedor e suprimidos (`app/ponto/services/cartorio.py:1681`; `app/ponto/precedencia.py:375-380`). |

Fontes adicionais da ata:

- Grade carrega EC/template, células da janela e FolgaDia (`app/escala/utils.py:1240-1260`, `:1356-1376`), batidas (`:1267`), aplica overlays FolgaDia/Ausencia/isenção (`:1785-1790`).
- FolgaDia altera tipo, limpa células/órfãs (`:1109-1120`); Ausencia vem de cobertura para folha, Justificativa aprovada do banco (`:1137-1159`, `:1170-1203`).
- Precedência consulta Ausencia não rejeitada, FolgaDia, feriados da praça e vínculos/células (`app/ponto/precedencia.py:227-245`, `:271-307`, `:332-360`).
- Não encontrei caminho comprovado de `AplicacaoConvencao`/régua CCT à produção da ata/status; atribuir essa fonte: `[nao verificado]`. A “régua” do veto do fingerprint vem de template degenerado/suspeito (`app/ponto/services/furos_vetados.py:33-43`).

Fingerprint:

`sha256('|'.join(partes).encode()).hexdigest()[:64]` (`app/ponto/services/cartorio.py:390`).

Partes obrigatórias: batidas `pk:tipo:timestamp.isoformat`, cobertura `str(status)`, chamados `pk:status_local`, **somente** `dna.dna_versao` concatenado com `dna.marcos` (`:333-338`). Opcionais: veto (`:339-340`), teto de marcos vencidos (`:344-345`), versão (`:352-353`), turno em curso `enc` (`:362-363`), quantidade de fios vivos (`:377-378`), parcial e estado de período (`:385-389`). Chamador normal fornece esses fatos (`:1104-1153`); nunca-bateu usa batidas/chamados vazios, cobertura, DNA, versão e parcial (`:998-999`). **Ata e demais colunas não são partes diretas** nesse corpo (`:333-389`).

Arestas principais, origem → destino:

- `dna → impressao`: `app/ponto/services/cartorio.py:337`.
- `data → impressao`: versão por competência e teto/turno (`:945-947`, `:1104-1153`).
- `ata.lampadas`, `ata.n_orfas`, `dna.marcos → tem_entrada/tem_saida`: `:643-665`.
- `veredito`, `veredito_via → status_dia/status_rotulo`: `app/ponto/portas/celula.py:694-698`; regra/rotulação `app/ponto/status_dia.py:64-121`.
- `protestos → veredito/status` no completar-status: `app/ponto/services/cartorio.py:733-735`.
- `trabalha → veredito`: `_protesta` `:1204-1208`; cadastro sem vínculo lê trabalha (`app/ponto/supra_juiz.py:170-174`).
- `trabalha → ata.precedencia.era_dia_previsto → demais precedencia`: `app/ponto/precedencia.py:305-340`, `:349-360`.
- `ata.precedencia anterior → ata nova`: preservação `app/ponto/portas/celula.py:730-734`.
- `ata.lampadas → ata.n_missing/n_celulas`: `app/ponto/services/cartorio.py:575`, `:591`.
- `ata.lampadas atuais → veredito`: `app/ponto/services/cartorio.py:1182-1208`, `:1244-1245`.

**Há ciclo temporal comprovado `ata anterior → ata nova`:**

1. Grade prévia chama `turnos_do_colab` alimentando células persistidas (`app/escala/utils.py:1519-1526`).
2. `turnos_do_colab` chama `papel_por_minuto_da_ata` (`app/ponto/turnos.py:1911`), que consulta `periodos_do_dia(cel,te)` (`:1629-1636`); juiz lê `ata/dna` (`app/ponto/juiz_batida.py:157-161`).
3. Grade passa turno ao matcher (`app/escala/utils.py:1643-1645`); matcher usa papéis para construir células (`:562-585`).
4. Cartório transforma células em ata persistida (`app/ponto/services/cartorio.py:564-567`, `:1557-1580`).

Também há dependência de **outra célula**: as luzes de todas são acumuladas e impedem tratar a luz vizinha como órfã (`app/ponto/turnos.py:1652`, `:1733-1735`); reconciliação pode retirar órfã de D+1 ao preencher saída de D (`app/escala/utils.py:1911-1913`, `:1936`, `:1979-1980`). Para calcular realizado, a grade já substitui atas por cópias com lâmpadas atuais em memória (`:1821-1838`), mas isso não remove a leitura anterior usada no pré-alinhamento (`:1519-1526`).

## 31/10 16:10 · sessao c2253669ab73 · - · - · origem=Ronald (tmux)

Escritores concretos encontrados, excluindo testes/migrations: [nao verificado]

| Coluna | Escritores e fontes provados |
|---|---|
| colaborador | `regenerar_celulas_vinculo`: `create(colaborador_id=ec.colaborador_id)` em `app/ponto/portas/celula.py:247`; `gerar_celulas_periodo`: `create` em `app/ponto/portas/celula.py:593`. |
| data | Mesmos `create`, data iterada na janela: `app/ponto/portas/celula.py:208`, `app/ponto/portas/celula.py:247`, `app/ponto/portas/celula.py:571`, `app/ponto/portas/celula.py:594`. |
| trabalha | Regenerador, inclusive limpeza fora da vigência: `app/ponto/portas/celula.py:231`, `app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:260`; gerador: `app/ponto/portas/celula.py:590`, `app/ponto/portas/celula.py:595`, `app/ponto/portas/celula.py:612`; reversor deriva de `dna_anterior.marcos`: `app/ponto/portas/celula.py:527`, `app/ponto/portas/celula.py:532`. |
| origem | `'gerada'` nos dois `create`: `app/ponto/portas/celula.py:248`, `app/ponto/portas/celula.py:595`; default em `app/escala/models.py:1659`. Escritor de `'editada'` não encontrado `[nao verificado]`. |
| escala_geradora | Regenerador: `app/ponto/portas/celula.py:248`, `app/ponto/portas/celula.py:260`; gerador: `app/ponto/portas/celula.py:595`, `app/ponto/portas/celula.py:613`; reversor usa `dna_anterior.vinculo_id`: `app/ponto/portas/celula.py:533`. Há `on_delete=SET_NULL`: `app/escala/models.py:1660`. |
| dna | Regenerador: `app/ponto/portas/celula.py:232`, `app/ponto/portas/celula.py:248`, `app/ponto/portas/celula.py:260`; gerador: `app/ponto/portas/celula.py:596`, `app/ponto/portas/celula.py:604`, `app/ponto/portas/celula.py:614`; reversor copia `dna_anterior`: `app/ponto/portas/celula.py:527`, `app/ponto/portas/celula.py:530`. |
| gerada_em | `auto_now_add` em `app/escala/models.py:1670`, acionado pelos `create` acima. |
| regenerada_em | Relógio em regeneração: `app/ponto/portas/celula.py:262`; relógio em reversão: `app/ponto/portas/celula.py:536`. |
| regeneracoes | Valor anterior +1 na regeneração: `app/ponto/portas/celula.py:263`; reversão: `app/ponto/portas/celula.py:537`. Default 0: `app/escala/models.py:1691`. |
| dna_anterior | DNA anterior à regeneração: `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:261`; limpo pelo reversor: `app/ponto/portas/celula.py:531`. |
| editada_em | Nenhum escritor encontrado `[nao verificado]`; o próprio serviço declara “escritor de producao: nenhum declarado hoje” em `app/escala/services/fase_12x36.py:161`. |
| editada_por | Nenhum escritor explícito encontrado `[nao verificado]`; deleção do usuário aplica `SET_NULL`: `app/escala/models.py:1701`. |
| julgada_em | `carimbar`: argumento `agora`, `app/ponto/portas/celula.py:800`, persistido via `salvar_e_emitir` em `app/ponto/portas/celula.py:807` → `app/ponto/portas/celula.py:640`. |
| impressao | `carimbar`: argumento `impressao`, `app/ponto/portas/celula.py:801`, mesmo save acima. |
| veredito | `lavrar_veredito`: `app/ponto/portas/celula.py:770`. Cartório deriva `veredito or derivar_veredito(codigos)` em `app/ponto/portas/celula.py:658`; admin fornece `veredito` em `app/ponto/portas/celula.py:750`. |
| veredito_via | `lavrar_veredito`: argumento via em `app/ponto/portas/celula.py:771`. |
| veredito_em | `lavrar_veredito`: argumento agora ou relógio em `app/ponto/portas/celula.py:772`. |
| protestos | `lavrar_veredito`: cópia dos códigos do cartório em `app/ponto/portas/celula.py:746`, ou preservação dos protestos anteriores no admin em `app/ponto/portas/celula.py:751`; atribuição em `app/ponto/portas/celula.py:769`. |
| ata | `lavrar_veredito`: copia argumento e preserva `precedencia` anterior se ausente no novo argumento em `app/ponto/portas/celula.py:730`; atribui em `app/ponto/portas/celula.py:741` ou `app/ponto/portas/celula.py:768`; `carimbar_precedencia` modifica chave em `app/ponto/portas/celula.py:819` e salva em `app/ponto/portas/celula.py:822`. |
| ausencia | `carimbar` argumento ausência em `app/ponto/portas/celula.py:802`; `carimbar_insumos_em_lote` em `app/ponto/portas/celula.py:838` + `bulk_update` em `app/ponto/portas/celula.py:843`. Deleção da ausência aplica `SET_NULL`: `app/escala/models.py:1764`. |
| tem_entrada | `carimbar`: argumento em `app/ponto/portas/celula.py:803`; lote em `app/ponto/portas/celula.py:839`, `app/ponto/portas/celula.py:843`. |
| tem_saida | Idem tem_entrada: `app/ponto/portas/celula.py:803`, `app/ponto/portas/celula.py:839`, `app/ponto/portas/celula.py:843`. |
| insumos_em | `carimbar`: agora em `app/ponto/portas/celula.py:804`; lote: agora em `app/ponto/portas/celula.py:840`, `app/ponto/portas/celula.py:843`. |
| status_dia | `_aplicar_status` em `app/ponto/portas/celula.py:705`, chamado por `lavrar_veredito` e salvo em `app/ponto/portas/celula.py:743`, `app/ponto/portas/celula.py:760`, `app/ponto/portas/celula.py:775`. |
| status_rotulo | Mesmo escritor e saves de status_dia: `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:743`, `app/ponto/portas/celula.py:760`, `app/ponto/portas/celula.py:775`. |

Fontes e eventos:

- `trabalha` vem de `EscalaColaborador.eh_dia_trabalho_calculado`, chamado pelo regenerador em `app/ponto/portas/celula.py:243` e gerador em `app/ponto/portas/celula.py:590`. A regra se declara sem leitura de célula em `app/escala/models.py:1226`; lê FolgaDia em `app/escala/models.py:1257`, âncora/ciclo em `app/escala/models.py:1260`, a foto em `app/escala/models.py:1280`, continuação da foto em `app/escala/models.py:1327`, regra do template em `app/escala/models.py:1337` e feriado por praça em `app/escala/models.py:1340`, `app/escala/models.py:1362`.
- `montar_dna` lê template, marcos/pausas em `app/ponto/management/commands/gerar_celulas.py:76`, `app/ponto/management/commands/gerar_celulas.py:81`; produz `dna_versao`, `vinculo_id`, `tipo_escala_id`, `tipo_ciclo`, `marcos`, `minutos_jornada`, `intervalo_livre`, `lampadas` em `app/ponto/management/commands/gerar_celulas.py:95`; extra declarada em `app/ponto/management/commands/gerar_celulas.py:119`.
- Gerador cron 05:50 em `app/config/crons.py:407`; o command chama o escritor em `app/ponto/management/commands/gerar_celulas.py:311`.
- Vigência alterada provoca regeneração em `app/escala/signals.py:34`, `app/escala/signals.py:71`; âncora alterada em `app/escala/signals.py:79`, `app/escala/signals.py:117`; foto FolgaDia em `app/escala/folgas.py:192`, `app/escala/folgas.py:220`; vínculo humano em `app/colaboradores/services/vinculo.py:320`; alteração template em `app/escala/services/cadastro_tipo.py:348`.
- Gerador normal respeita origem editada e passado, mas enriquece DNA nulo passado em `app/ponto/portas/celula.py:598`–`app/ponto/portas/celula.py:605`; regrava futuro em `app/ponto/portas/celula.py:609`–`app/ponto/portas/celula.py:615`. Não há consulta de competência exportada nesse corpo: `app/ponto/portas/celula.py:555`–`app/ponto/portas/celula.py:617`.
- Regenerador usa `_competencias_exportadas` em `app/ponto/portas/celula.py:199`, barra em `app/ponto/portas/celula.py:238`, mas `apesar_da_lavra` ignora essa guarda em `app/ponto/portas/celula.py:196`. Reversor tem mesma guarda em `app/ponto/portas/celula.py:520`. `_competencias_exportadas` chama `competencia_fechada_para_celula` em `app/ponto/management/commands/gerar_celulas.py:173`; este combina TXT vigente (`competencia_entregue`) e holerite próprio com lote publicado em `app/folha/export.py:599`–`app/folha/export.py:606`. Não usa FechamentoMensal.status, conforme documentação em `app/ponto/management/commands/gerar_celulas.py:132`.
- Lavra humana do veredito em `app/ponto/services/veredito_celula.py:330`; ela não escreve origem/editada_em/editada_por (chamada explícita somente de lavrar_veredito nessa linha).
- Lote backfill: ausência de `ausencia_cobre`, flags de `insumos_da_ata(cel.ata, cel.dna)` em `app/ponto/management/commands/backfill_b5_insumos.py:72`; chama carimbo lote em `app/ponto/management/commands/backfill_b5_insumos.py:81`. Nenhum portão de competência no corpo `app/ponto/management/commands/backfill_b5_insumos.py:51`–`app/ponto/management/commands/backfill_b5_insumos.py:84`.
- Status deriva do veredito/via em `app/ponto/portas/celula.py:694`–`app/ponto/portas/celula.py:698`. Admin usa mapa em `app/ponto/status_dia.py:52`, aplicado em `app/ponto/status_dia.py:64`. Cartório usa fatos da grade/batidas/realizado/isento/cobertura parcial/precedência em `app/ponto/status_dia.py:124`–`app/ponto/status_dia.py:149`, regra em `app/ponto/status_dia.py:96`–`app/ponto/status_dia.py:121`.
- `lavrar_status_competencia` barra competência entregue em `app/ponto/management/commands/lavrar_status_competencia.py:40`; chama `julgar_colab(completar_status=True)` em `app/ponto/management/commands/lavrar_status_competencia.py:55`.

Arestas especialmente relevantes:

- `dna → dna_anterior` na regeneração: `app/ponto/portas/celula.py:259`–`app/ponto/portas/celula.py:261`.
- `dna_anterior → dna`, `dna_anterior → trabalha`, `dna_anterior → escala_geradora` na reversão: `app/ponto/portas/celula.py:527`–`app/ponto/portas/celula.py:535`.
- Portanto há ciclo operacional `dna ↔ dna_anterior` se o grafo inclui eventos sucessivos de regeneração/reversão; a reversão zera dna_anterior em `app/ponto/portas/celula.py:531`, evitando ping-pong conforme documentação em `app/ponto/portas/celula.py:497`.
- `regeneracoes → regeneracoes` incremento: `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:537`.
- `protestos → protestos` preservação pelo admin: `app/ponto/portas/celula.py:751`.
- `ata.precedencia → ata.precedencia` preservação por lavra: `app/ponto/portas/celula.py:732`.
- `veredito/veredito_via → status_dia/status_rotulo`: `app/ponto/portas/celula.py:694`–`app/ponto/portas/celula.py:698`.

## 31/10 16:09 · sessao 12e38d744b15 · GRADE-FOLGA-DO-VINCULO · leitura · origem=Code

**O horizonte da versão 3 controla a impressão que provoca o rejulgamento; ele não seleciona uma implementação antiga da regra de folga.** Quando um dia anterior ao horizonte é rejulgado, o cartório usa a grade com a regra nova. O filtro que recupera a folga da ata antiga pertence à leitura do espelho e não ao caminho do cartório. (`app/ponto/services/cartorio.py:948`, `:962–965`, `:1165–1178`; `app/escala/utils.py:1163–1171`, `:2329–2330`.)

Todas as referências abaixo são de **ea38de71**, salvo indicação **B/c7ea207a**. A reprodução das contagens e a atribuição causal de cada uma das 16 células anteriores a 21/09 ficam **[nao verificado]**.

**1. Caminho que decide `ata.tipo_dia`**

O caminho é:

1. O comando carrega **todas as células passadas** do colaborador e chama `julgar_colab`, repassando `--forcar`. (`app/ponto/management/commands/processar_cartorio.py:49–65`.)
2. O serviço chama diretamente `montar_grade_prevista_periodo`. O dispatcher escolhe o builder por turno ou o legacy. (`app/ponto/services/cartorio.py:948`; `app/escala/utils.py:2076–2082`.)
3. No builder por turno, a grade recebe o tipo inicial `folga` ou `trabalho`, depois os overlays de folga lançada, ausência e isenção. (`app/escala/utils.py:1794–1804`, `:1837–1842`.)
4. `ata_do_dia` copia literalmente `dia.get('tipo_dia')`. O cartório entrega essa ata à lavra e persiste o resultado pelo carimbo. **Não é `classificar_dia` que escolhe esse campo da ata.** (`app/ponto/services/cartorio.py:584`, `:1574–1581`, `:1595–1597`.)

**Esse caminho usa a regra nova.** Em B, o overlay buscava qualquer `FolgaDia` do vínculo recebido e era repetido para cada vínculo da janela. Em C, carrega todas as folgas da pessoa e aceita a primeira válida para o vínculo daquele dia: manual vale independentemente do vínculo; calendário exige o vínculo do dia. (**B:** `app/escala/utils.py:1109–1120`, `:1785–1788`; **C:** `app/escala/utils.py:1119–1125`; `app/ponto/precedencia.py:207–219`, `:236–240`.)

**Não há consulta a `versao_para`, `VERSAO_GRADE_FOLGA` ou `REGRA_VERSOES` para escolher a regra de folga nesse overlay.** A versão é calculada separadamente para compor a impressão. O teste `< VERSAO_GRADE_FOLGA` ocorre em `_ata_abaixo_da_regra_de_folga`, chamado por `grade_espelho_janela`, que o cartório não usa. (`app/escala/utils.py:1114–1125`, `:1163–1171`, `:2330`; `app/ponto/services/cartorio.py:948`, `:962–965`.)

A alteração de `colaboradores/services/calendario.py` pertence à apresentação do calendário, incluindo `eh_trabalho`; ela não integra esse caminho de construção da ata. (`app/colaboradores/services/calendario.py:93–105`, `:617–623`, `:838–840`; `app/ponto/services/cartorio.py:948`.) `escala/folgas.py` já preserva a folga de calendário no vínculo original. (`app/escala/folgas.py:405–406`.) Sua ausência de alteração entre os commits foi conferida pelo diff Git, mas não tem prova isolada em arquivo:linha: **[nao verificado]** nesse formato.

**2. Com e sem `--forcar`**

**Com `--forcar`, sim: dias com `versao_para < 3` são rejulgados.** A flag desativa o pulo por impressão igual, tanto no ramo normal quanto no ramo sem nenhuma batida na vida; não existe condição de versão nessa decisão. (`app/ponto/services/cartorio.py:1017–1023`, `:1174–1183`.)

Rejulgar não garante alteração dos valores. Mesmo uma decisão administrativa preserva o veredito contra o cartório, mas permite atualizar ata e status. (`app/ponto/portas/celula.py:735–745`.)

**Sem `--forcar`, o cron declarado executa `--apply`.** No ramo normal, uma célula é pulada somente quando tem `julgada_em`, impressão igual, veredito preenchido e veredito diferente de `nunca_bateu`. Logo, impressão divergente, ausência de julgamento/veredito ou `nunca_bateu` após adesão permitem novo julgamento, inclusive na competência 2026-09. (`app/config/crons.py:597`; `app/ponto/services/cartorio.py:1171–1183`.)

Na competência 2026-09, `versao_para` continua **1**: a função escolhe a maior versão cujo horizonte alcança a competência, e as versões 2 e 3 começam em 2026-10. Isso mantém apenas o componente `regra:1`; não congela os demais componentes. (`app/ponto/services/cartorio.py:110`, `:208–214`, `:369–370`.)

A impressão contém:

| Componente | Conteúdo |
|---|---|
| Batidas | `pk`, tipo e timestamp das batidas apuráveis de D−1 a D+1 |
| Cobertura | Status da ausência para cobrança |
| Chamados | `pk` e `status_local` dos chamados de módulos de batida |
| DNA | `dna_versao` e `marcos` |
| Condicionais | Veto, teto de marcos vencidos, versão da regra, turno em curso, quantidade de chamados vivos com fio e estado da cobertura parcial |

Fontes: `app/ponto/services/cartorio.py:350–407`, `:943–945`, `:1122–1125`, `:1151–1170`; seleção dos chamados em `:90–105`.

**`tipo_dia`, a ata inteira e `status_dia` não são componentes diretos do hash.** Há, porém, componentes derivados da grade, especialmente `periodo_parcial`. (`app/ponto/services/cartorio.py:350–407`, `:1170`.)

Mesmo sem flag no cron, há segunda passada interna com `forcar=True` para células reconciliadas. (`app/ponto/services/cartorio.py:803–818`.)

**3. A impressão abaixo da versão 3 pode mudar?**

**Sim, pelo componente `cobparcial`, mesmo mantendo `regra:1` e o mesmo `tipo_dia` final.** O mecanismo comprovado é:

- O overlay de folga limpa células e órfãs. (`app/escala/utils.py:1132–1134`.)
- A ausência parcial mantém essas listas e calcula `periodo_parcial` pelas horas disponíveis. (`app/escala/utils.py:1231–1248`.)
- Essas horas vêm das células cumpridas e órfãs; havendo alguma dentro do período declarado, o resultado é `dentro`; sem nenhuma, `fora`. (`app/ponto/turnos.py:2611–2614`, `:2640–2647`.)
- O cartório incorpora esse estado como `cobparcial:1:<estado>`, sem filtro por versão. (`app/ponto/services/cartorio.py:402–407`, `:1170`.)

Portanto, retirar uma folga inválida pode preservar batidas, mudar `fora→dentro` e alterar a impressão, enquanto B e C terminam com `tipo_dia='ausencia'`. Essa é uma consequência do código; sua ocorrência nas células informadas está **[nao verificado]**. (`app/escala/utils.py:1124–1125`, `:1231–1245`.)

Também mudou a ordem dos overlays: B fazia folga→ausência **por vínculo**; C aplica a folga uma vez, antes de todas as ausências. Isso pode alterar células e órfãs sem exigir mudança do tipo final. (**B:** `app/escala/utils.py:1785–1788`; **C:** `app/escala/utils.py:1837–1840`.)

**4. Consumidores da competência 2026-09 exportada**

**Folha e cálculo online.** `grade_da_celula` prefere `ata.tipo_dia` ao campo `trabalha`, e entrega também minutos, lâmpadas, órfãs e veredito. A folha usa esse leitor na janela da competência; `trabalho` e `ausencia` entram no universo de dias previstos. Assim, `folga→trabalho` pode mudar previsto, realizado considerado, cobertura e pendências. O efeito numérico dessas células está **[nao verificado]**. (`app/escala/services/leitor_celula.py:325–365`; `app/folha/export.py:318–327`; `app/ponto/services/fechamento.py:1014–1035`.)

A competência exportada **não congela essa leitura online**: `fechamento_lido` recalcula em memória; a guarda de escrita depende da lista de exportadas, que não é carregada no modo de leitura puro. Os totais de tela/cartão usam essa leitura. (`app/ponto/services/fechamento.py:523–548`, `:1727–1729`; `app/relatorios/cartao_pela_celula.py:69–76`.)

**Espelho.** O caminho principal lê a grade da ata e deriva dela o indicador de folga. Uma ata alterada pode retirar esse indicador; o status visual depende também de ausência, inconsistência, batidas, isenção e previsto. Há fallback para o builder em atas agregadas sem marcos. (`app/ponto/services/espelho.py:529–548`, `:690`, `:152–171`.) No fallback, o filtro abaixo da versão 3 só restaura folga **se a ata ainda disser folga**; uma ata já alterada para trabalho não é recuperada por ele. (`app/escala/utils.py:1160–1171`.)

**Novo export.** Os eventos monetários são montados a partir dos fechamentos, mas a retenção consulta os vereditos e protestos atuais das células. Portanto, mudar ata não prova mudança imediata das quantias; mudar veredito/protestos pode alterar quem entra em um novo TXT. (`app/folha/export.py:695–725`, `:942–946`, `:1188`, `:1215–1238`.)

**Export já gravado.** O download devolve `reg.conteudo`; o PDF do TXT traduz esse registro salvo. Esses downloads não são regenerados a partir da ata atual. (`app/folha/views.py:548–563`, `:818–834`.)

**`CelulaDia.status_dia` é distinto do status visual do espelho.** Existe leitor explícito do campo persistido, mas os caminhos acima calculam suas apresentações pelas fontes descritas. A ausência universal de leitores indiretos ou dinâmicos fica **[nao verificado]**. (`app/ponto/status_dia.py:171–185`; `app/ponto/services/espelho.py:152–183`.)

**5. Outra mudança capaz de explicar os dias anteriores a 21/09**

Na inspeção do diff, não encontrei alteração independente da correção de folga no classificador, hash, reconciliação ou lavra. Esse inventário negativo foi conferido pelo Git, mas não possui prova isolada por arquivo:linha: **[nao verificado]** nesse formato.

Há efeitos da própria correção além de `tipo_dia`: a precedência passou de guardar somente a primeira folga por data e eventualmente descartá-la para guardar todas e escolher a primeira válida; além disso, há a mudança de ordem dos overlays e o efeito sobre cobertura parcial descritos acima. (**B:** `app/ponto/precedencia.py:234–238`, `:313–315`; **C:** `app/ponto/precedencia.py:207–219`, `:358`; `app/escala/utils.py:1837–1840`, `:1245`.)

**B e C sucessivos no mesmo banco também não isolam apenas a troca de código:** o julgamento lê chamados/fios atuais, pode fechar chamados e criar perguntas; há segunda passada e releitura após emissão. Assim, parte das diferenças pode decorrer do estado deixado pela rodada B. Atribuir esse efeito às oito células restantes exige comparar seus insumos e chamados/perguntas entre as fases: **[nao verificado]**. (`app/ponto/services/cartorio.py:1086`, `:1151–1156`, `:1298–1305`, `:840–845`, `:814–818`, `:870–883`.)

## 31/10 16:08 · sessao d2e4aeb966ef · - · - · origem=Ronald (tmux)

Q5: a inspeção de `git diff c7ea207a..ea38de71` encontrou quatro módulos de produção alterados: `calendario.py`, `escala/utils.py`, `precedencia.py` e `cartorio.py`; não apareceu alteração independente em classificação, serialização de chamados, reconciliação ou status. O inventário completo do diff não possui um `arquivo:linha` próprio: [nao verificado por esse formato de referência].

Há, porém, efeitos além da troca explícita de `tipo_dia`:

- **Precedência com múltiplas folgas:** B guardava somente a primeira `FolgaDia` por data e depois podia descartá-la (`c7ea207a:app/ponto/precedencia.py:234`, `:313`). C conserva todas e escolhe a primeira válida (`app/ponto/precedencia.py:207`, `:217`, `:358`). Isso pode mudar a precedência mesmo quando o tipo final da grade permanece igual; a precedência determina os fatos vencedores em `app/ponto/precedencia.py:380` e `:392`.
- **Ausência parcial com mesmo `tipo_dia='ausencia'`:** aplicar folga limpa células e órfãs (`app/escala/utils.py:1132`). A ausência parcial preserva essas listas e calcula `periodo_parcial` pelas horas disponíveis (`app/escala/utils.py:1231`, `:1241`, `:1245`). Retirar uma folga inválida pode preservar batidas e alterar `fora`/`dentro`, sem trocar o tipo final; as horas vêm das células e órfãs (`app/ponto/turnos.py:2640`), e a decisão do período está em `app/ponto/turnos.py:2611`. Esse estado entra na impressão (`app/ponto/services/cartorio.py:1170`, `:402`).
- **Estado deixado pela fase B:** o julgamento lê chamados e perguntas atuais (`app/ponto/services/cartorio.py:1086`, `:1151`, `:1224`), pode reconciliar chamados (`:1298`, `:1332`) e criar perguntas (`:840`). Existe segunda passada forçada (`:814`) e releitura após emissão (`:870`), mas sobras posteriores são descartadas com contador (`:823`). Assim, diferenças B→C podem conter efeitos do estado já modificado por B. Atribuir isso aos oito dias concretos é [nao verificado].

O novo `trabalho_do_dia` pertence à apresentação do calendário (`app/colaboradores/services/calendario.py:93`, `:838`). O gate que recupera a ata antiga pertence a `grade_espelho_janela` (`app/escala/utils.py:1163`, `:1170`, `:2330`); o cartório chama diretamente o builder (`app/ponto/services/cartorio.py:948`).

O cron declarado executa `processar_cartorio --apply`, sem `--forcar`: `app/config/crons.py:597`.

## 31/10 16:08 · sessao 34395c524c81 · - · - · origem=Ronald (tmux)

O caminho do cartório usa a regra nova de folga **antes de qualquer decisão sobre pular a célula**, sem selecionar implementação velha conforme `versao_para`: `app/ponto/services/cartorio.py:948`, `app/escala/utils.py:1837`, `app/escala/utils.py:1114`, `app/escala/utils.py:1124`, `app/ponto/services/cartorio.py:1178`.

Caminho comprovado:

- `processar_cartorio` carrega todas as células passadas do colaborador e chama `julgar_colab`, repassando `--forcar`: `app/ponto/management/commands/processar_cartorio.py:61`, `app/ponto/management/commands/processar_cartorio.py:65`.
- `_julgar_colab_corpo` monta a grade com `montar_grade_prevista_periodo`: `app/ponto/services/cartorio.py:948`. Esse dispatcher seleciona builder por turno ou legacy: `app/escala/utils.py:2076`. A configuração default da flag é `True`; valor efetivo do ambiente [nao verificado]: `app/config/settings/base.py:165`.
- O builder por turno determina vínculo de cada data pela juíza, alimentada com vínculos e células: `app/escala/utils.py:1291`, `app/escala/utils.py:1303`, `app/escala/utils.py:1305`, `app/escala/utils.py:1311`.
- O tipo inicial é `folga` ou `trabalho`: `app/escala/utils.py:1794`, `app/escala/utils.py:1800`. Depois aplica folga lançada, ausências e isenção: `app/escala/utils.py:1837`, `app/escala/utils.py:1840`, `app/escala/utils.py:1842`.
- A folga lançada carrega **todas** as `FolgaDia` da pessoa no período: `app/escala/utils.py:1119`, `app/ponto/precedencia.py:207`. Para cada dia, aceita a primeira candidata válida: `app/escala/utils.py:1124`, `app/ponto/precedencia.py:217`. Manual vale sempre; calendário exige o vínculo do dia: `app/ponto/precedencia.py:236`, `app/ponto/precedencia.py:238`, `app/ponto/precedencia.py:240`.
- Overlay escreve `tipo_dia='folga'`, limpa células e órfãs: `app/escala/utils.py:1132`.
- `ata_do_dia` copia literalmente `dia.get('tipo_dia')`: `app/ponto/services/cartorio.py:584`. A lavra recebe essa ata e os fatos do status; `carimbar` persiste o resultado: `app/ponto/services/cartorio.py:1574`, `app/ponto/services/cartorio.py:1576`, `app/ponto/services/cartorio.py:1580`, `app/ponto/services/cartorio.py:1596`.

O gate com `versao_para < VERSAO_GRADE_FOLGA` existe no **overlay de leitura de ata antiga**, e só transforma grade viva `trabalho` em `folga` se a ata ainda disser `folga`: `app/escala/utils.py:1160`, `app/escala/utils.py:1163`, `app/escala/utils.py:1169`, `app/escala/utils.py:1170`. Quem o chama é `grade_espelho_janela`, depois do builder: `app/escala/utils.py:2329`, `app/escala/utils.py:2330`. O cartório chama o builder diretamente, portanto não passa por esse gate: `app/ponto/services/cartorio.py:948`.

Comparação confirmada pelo Git local (`git -C /home/ronald/wt-v3r2b-133946 diff c7ea207a..ea38de71`):

- Em B, `_aplicar_folgas_foto` consultava somente o vínculo passado e aceitava qualquer FolgaDia dele, sem verificar vínculo do dia; no builder por turno isso era repetido para cada vínculo da janela: **c7ea207a**, `app/escala/utils.py:1101`, `app/escala/utils.py:1109`, `app/escala/utils.py:1116`, `app/escala/utils.py:1785`, `app/escala/utils.py:1787`.
- Em C, a regra é a seleção por pessoa e vínculo do dia descrita acima: **ea38de71**, `app/escala/utils.py:1119`, `app/escala/utils.py:1124`, `app/escala/utils.py:1837`.
- `escala/folgas.py` não mudou nesse diff [nao verificado por arquivo:linha; comprovado por diff Git]. Seu comportamento já preservava calendário no vínculo original: `app/escala/folgas.py:405`, `app/escala/folgas.py:406`.
- Na precedência, além de extrair o predicado, C passou a guardar todas as candidatas por dia e testar cada uma; B guardava só a primeira, podendo descartar uma candidata válida posterior: **C**, `app/ponto/precedencia.py:207`, `app/ponto/precedencia.py:209`, `app/ponto/precedencia.py:217`; **B**, `app/ponto/precedencia.py:235`, `app/ponto/precedencia.py:238` — estas últimas linhas precisam ser conferidas novamente antes de usar [nao verificado].
- `colaboradores/services/calendario.py` deixou de fornecer à precedência `{data: True}` apenas do vínculo ativo, permitindo o carregador completo: `app/colaboradores/services/calendario.py:617`. Também passou a publicar `eh_trabalho` com o mesmo juiz utilizado na classificação: `app/colaboradores/services/calendario.py:93`, `app/colaboradores/services/calendario.py:839`, `app/colaboradores/services/calendario.py:840`. Esse arquivo não integra o caminho de construção da ata pelo cartório descrito acima: `app/ponto/services/cartorio.py:948`, `app/escala/utils.py:2076`.

Há ainda uma mudança de **ordem dos overlays** dentro da mesma alteração de folga: em B, para cada vínculo aplicava folga e depois ausência, permitindo que folga de um vínculo posterior limpasse células/órfãs de uma ausência já aplicada; em C, aplica a folga única antes de todas as ausências: **B**, `app/escala/utils.py:1785`, `app/escala/utils.py:1787`, `app/escala/utils.py:1788`; **C**, `app/escala/utils.py:1837`, `app/escala/utils.py:1838`, `app/escala/utils.py:1840`. Se isso explica alguma das oito células sem mudança de tipo, [nao verificado] sem os respectivos insumos.

A mudança pode afetar `status_dia` indiretamente: seus fatos vêm do mesmo `dia`, inclusive tipo, previsto, células, órfãs e realizado: `app/ponto/status_dia.py:143`, `app/ponto/status_dia.py:146`. O status usa `folga` para dia sem batida e tipo folga ou previsto zero; com batida e tipo folga, usa `folga_trabalhada`: `app/ponto/status_dia.py:105`, `app/ponto/status_dia.py:106`, `app/ponto/status_dia.py:115`.

## 31/10 16:08 · sessao d0f45fcacb67 · - · - · origem=Ronald (tmux)

Conclusões no snapshot `ea38de71`, somente leitura:

- **Leitor direto de `ata.tipo_dia`:** `grade_da_celula` consulta todas as `CelulaDia` da janela, lê a ata e prefere `ata['tipo_dia']`; só usa `cel.trabalha` se a chave não existe. Entrega também minutos, lâmpadas, órfãs e veredito. Não há consulta a exportação ou versão nessa função. (`app/escala/services/leitor_celula.py:321-371`, especialmente `:325`, `:331`, `:344-365`.)

- **Folha:** `grade_do_fechamento` chama esse leitor na janela da competência (`app/folha/export.py:318-327`). Um `folga → trabalho` muda a inclusão no universo de dias previstos e na classificação de causa/furos (`app/folha/causas.py:39-45`, `:58-66`, `:92-108`). Na derivação de folha, `trabalho` e `ausencia` entram em previsto/realizado; folga fica fora (`app/ponto/services/fechamento.py:1014-1035`; a derivação por dia está em `app/ponto/services/dia_pago.py:384-398`). Não se pode afirmar quanto mudaria sem examinar os minutos/lâmpadas das células específicas: **[não verificado]**.

- **A guarda da competência 09 não congela leitura online.** A guarda lista `(9, 2026)` (`app/ponto/services/fechamento.py:36-43`). A lista `_exportadas` só é carregada quando há escrita, ou lavratura de DiaPago (`:523-524`); a exclusão pela guarda exige `_guarda and _exportadas` (`:544-548`). `fechamento_lido` calcula com `somente_leitura=True` e devolve objeto sem salvar (`:1703-1729`). Tela e PDF usam essa leitura para os totais da folha (`app/relatorios/cartao_pela_celula.py:69-76`, `:320-323`). Assim, uma ata alterada pode repercutir no cálculo online da competência 09 exportada, embora a escrita dos registros monetários continue protegida pelos ramos indicados.

- **Espelho web:** a montagem obtém a grade da ata a partir de `max(data_ini, apur_ini)` (`app/ponto/services/espelho.py:517-538`); o trecho visual anterior vem do builder (`:539-547`). A classificação da grade alimenta `folga` (`:684-690`, `:779-782`). Com a ata mudada, pode desaparecer o badge “Folga” (`app/templates/ponto/espelho.html:160-166`), e, na ausência de batidas, ausência, inconsistência ou isenção, o status deixa de seguir o ramo `folga` e passa a `sem_registro` se houver previsto passado, senão `vazio` (`app/ponto/services/espelho.py:152-171`). A exceção para ata agregada sem marcos recorre ao builder, portanto não cabe prometer efeito uniforme em todo espelho (`:548-571`).

- **PDF de espelho:** lê a lista de dias de `espelho_do_colab` e a filtra pela janela pedida (`app/relatorios/pdf_espelho.py:592-599`). Reaplica a palavra do dia com a lista de dias em aberto do papel (`:624-634`); a coluna observações imprime `palavra_dia` (`:912-920`). A palavra é produzida com fatos de precedência, decisão/cobertura e dias em aberto, e não simplesmente com `ata.tipo_dia` (`app/ponto/services/espelho.py:339-356`, `:655-660`). Logo “mudou tipo_dia ⇒ PDF necessariamente imprime Trabalho” não está provado: **[não verificado]**.

- **Novo export:** `montar_linhas_export` lê os `FechamentoMensal` gravados, classifica quem entra e emite os eventos desses fechamentos (`app/folha/export.py:692-727`). Os valores de eventos saem de campos monetários do fechamento (`:409-441`), portanto alterar só a ata não prova alteração imediata das quantias emitidas. Entretanto o filtro de retenção lê as células atuais da competência: `classificar_export` chama o retentor (`:942-946`), que consulta `CelulaDia` e usa veredito e protestos (`:1188`, `:1215-1238`). Se o cartório alterar veredito/protestos, pode mudar quem entra ou fica de fora do **novo** TXT, independentemente de números gravados idênticos.

- **Export existente:** baixar o TXT entrega `reg.conteudo`, sem regerar (`app/folha/views.py:548-563`). O PDF do TXT também traduz o registro já salvo (`:818-835`). Portanto mudanças em ata/veredito não alteram esses downloads existentes.

- **`status_dia` é outro campo.** O leitor `status_do_dia_lavrado` consulta e devolve `CelulaDia.status_dia/status_rotulo`, com precedência do relógio antes do encerramento (`app/ponto/status_dia.py:171-185`). A busca global de produção não encontrou chamada dessa função fora de sua definição e do registro em `app/core/juizes.py:173`; folha e espelho examinados usam os caminhos acima. A ausência universal de qualquer leitura dinâmica/indireta é **[não verificado]**. Não confundir `CelulaDia.status_dia` com o `dia['status']` que `_status_dia` monta no espelho (`app/ponto/services/espelho.py:152-183`).

## 31/10 16:08 · sessao 92cb4569b890 · TRAVA-COBERTURA · censo · origem=Code

A cobertura do selo é incompleta: **calendário e autocura executam a lista inteira sem a trava**, e esses disparos não são cobrados por ele (`bin/regua_calendario.sh:22`, `bin/regua_calendario.sh:45`, `bin/molde_autocura/rodar.sh:25`, `bin/molde_autocura/rodar.sh:26`, `bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:59`).

Nos quadros, **CI** significa `config.settings.ci`: host `DB_HOST` ou `juliani_db_test`; banco de teste `REGUA_DB` ou `test_juliani` (`app/config/settings/ci.py:12`, `app/config/settings/ci.py:17`). Os valores efetivos vindos dos env-files/imagem em execução estão **[nao verificado]**. “Inteira” significa a lista `LABELS` da régua; `tenants` roda separado (`bin/regua.sh:23`, `CLAUDE.md:197`).

**Disparos em `bin/`:**

| Arquivo:linha do disparo | Passa pela trava? | Banco/settings | Quem chama | Escopo |
|---|---|---|---|---|
| `bin/suite.sh:102` | **Sim**, direto: `bin/suite.sh:96` | CI; repassa `REGUA_DB` exportado: `bin/suite.sh:89`; sem ele, default CI | Humano, comando canônico: `CLAUDE.md:171` | Inteira ou recorte/teste indicado por `--only`: `bin/suite.sh:47`, `bin/suite.sh:102` |
| `bin/regua.sh:168` | **Sim**, direto: `bin/regua.sh:165` | CI; default declarado: `bin/regua.sh:159`; settings: `bin/regua.sh:169` | Integrador: `app/core/integrador_lote.py:375`; cadeias nas linhas abaixo | Inteira ou `--only`: `bin/regua.sh:23`, `bin/regua.sh:51` |
| `bin/pre-push.sh:186` | **Sim**, por `_teste_irmao`: `bin/pre-push.sh:142` | CI, sem `REGUA_DB` explícito: `bin/pre-push.sh:186`, `bin/pre-push.sh:188` | Wrapper de hook produzido por `bin/hooks.sh:56`; instalação efetiva **[nao verificado]** | Inteira, `$LABELS`: `bin/pre-push.sh:184`, `bin/pre-push.sh:187` |
| `bin/pre-push.sh:198` | **Sim**, mesmo wrapper: `bin/pre-push.sh:142` | `teste_motor`: `bin/pre-push.sh:199`; host fixo `juliani_db_test`, conexão `juliani`: `app/config/settings/teste_motor.py:6`, `app/config/settings/teste_motor.py:7`; nome final do banco criado pelo Django **[nao verificado]** | Mesmo hook: `bin/hooks.sh:56` | Recorte `tenants`: `bin/pre-push.sh:198` |
| `bin/regua_relogio.sh:55` | **Sim**, direto: `bin/regua_relogio.sh:51` | Shim `cfg_tz` herda CI: `bin/regua_relogio.sh:35`; sem `REGUA_DB` explícito: `bin/regua_relogio.sh:52` | Comentário declara cron: `bin/regua_relogio.sh:48`; inscrição atual em `config/crons.py` **[nao verificado]** | Inteira, `APPS=LABELS`: `bin/regua_relogio.sh:27`, `bin/regua_relogio.sh:29` |
| `bin/vigia_arvore.sh:86` | **Sim**, direto: `bin/vigia_arvore.sh:83` | CI; `test_juliani_vigia` na primeira rodada: `bin/vigia_arvore.sh:88`; `test_juliani_vigia_b` na repetição: `bin/vigia_arvore.sh:106` | Cron horário: `app/config/crons.py:277`; também `_revigiar`: `app/core/esteira_vigia.py:1141` | Inteira primeiro; recorte das falhas depois: `bin/vigia_arvore.sh:88`, `bin/vigia_arvore.sh:105`, `bin/vigia_arvore.sh:106` |
| `bin/vigia_arvore.sh:134` | **Sim**, direto: `bin/vigia_arvore.sh:131` | CI; `REGUA_DB=test_juliani_vigia_viva`: `bin/vigia_arvore.sh:132` | Mesmo cron/revigia acima; ramo interno após repetição: `bin/vigia_arvore.sh:113` | Recorte `$IDS2`: `bin/vigia_arvore.sh:113`, `bin/vigia_arvore.sh:134` |
| `bin/molde_fatia/rodar.sh:30` | **Não**, Docker direto: `bin/molde_fatia/rodar.sh:28` | CI; `REGUA_DB=test_juliani___FATIA__m`: `bin/molde_fatia/rodar.sh:29` | Esteira: `bin/molde_fatia/esteira.sh:15`; refabricação de `rodar.sh` gerado: `bin/refabricar.sh:59` | Recorte de um módulo, `core.tests.test_selo_mapa_contrato`: `bin/molde_fatia/rodar.sh:30` |
| `bin/molde_fatia/rodar.sh:41` | **Sim**, direto: `bin/molde_fatia/rodar.sh:40` | CI; RED `test_juliani___FATIA__r`, GREEN `test_juliani___FATIA__g`: `bin/molde_fatia/rodar.sh:46`, `bin/molde_fatia/rodar.sh:47` | Mesma esteira: `bin/molde_fatia/esteira.sh:15` | Recortes RED/GREEN; retirada da inteira é explícita: `bin/molde_fatia/rodar.sh:46`, `bin/molde_fatia/rodar.sh:49` |
| `bin/molde_autocura/rodar.sh:20` | **Não**, Docker direto: `bin/molde_autocura/rodar.sh:20` | CI; RED `test_juliani_esacred`, GREEN `test_juliani_esacfull`: `bin/molde_autocura/rodar.sh:24`, `bin/molde_autocura/rodar.sh:26` | Esteira de autocura: `bin/molde_autocura/esteira.sh:14`; pacote gerado pelo vigia: `app/core/esteira_vigia.py:1426` | RED recortado por IDs; **GREEN inteiro**: `bin/molde_autocura/rodar.sh:22`, `bin/molde_autocura/rodar.sh:25`, `bin/molde_autocura/rodar.sh:26` |
| `bin/isolamento.sh:33` | **Não**, Docker direto: `bin/isolamento.sh:31` | CI; `test_juliani_iso_$fatia`: `bin/isolamento.sh:45`, `bin/isolamento.sh:58` | Cadeias: `bin/molde_fatia/cadeia.sh:55`, `bin/molde_autocura/cadeia.sh:52` | Recorte dos IDs falhos: `bin/isolamento.sh:20`, `bin/isolamento.sh:40` |
| `bin/regua_calendario.sh:45` | **Não**, Docker direto: `bin/regua_calendario.sh:43` | Shim herda CI: `bin/regua_calendario.sh:30`; `test_juliani_cald20`, `test_juliani_cald21`, `test_juliani_cald01`: `bin/regua_calendario.sh:41`, `bin/regua_calendario.sh:44` | Chamador atual **[nao verificado]** | **Inteira três vezes**, com datas diferentes: `bin/regua_calendario.sh:22`, `bin/regua_calendario.sh:41`, `bin/regua_calendario.sh:45` |
| `bin/placar_code.sh:124` | **Não**, Docker direto: `bin/placar_code.sh:122` | CI; `REGUA_DB=test_juliani_placar`: `bin/placar_code.sh:123`, `bin/placar_code.sh:126` | Chamador atual **[nao verificado]** | Recorte de uma classe, `CContratoCredencialSemTrilhaTest`: `bin/placar_code.sh:125` |

Os nomes com `__FATIA__` são moldes substituídos pelo gerador; os arquivos materializados em execução estão **[nao verificado]** (`bin/molde_fatia/gerar.py:24`, `bin/molde_fatia/gerar.py:29`, `bin/molde_fatia/gerar.py:32`).

**Disparos em `app/`:**

O executor comum `_rodar_testes` monta `manage.py test` em `app/core/esteira_vigia.py:757` e executa o subprocesso diretamente em `app/core/esteira_vigia.py:760`, **sem `trava_teste.sh`**. Injeta o argumento `banco` em `REGUA_DB` e usa CI (`app/core/esteira_vigia.py:752`, `app/core/esteira_vigia.py:758`).

| Arquivo:linha do disparo | Passa pela trava? | Banco/settings | Quem chama | Escopo |
|---|---|---|---|---|
| `app/core/hooks/pre-push.sh:82` | **Não**, helper Docker direto: `app/core/hooks/pre-push.sh:54`; outro lock em `app/core/hooks/pre-push.sh:32` | CI, sem `REGUA_DB` explícito: `app/core/hooks/pre-push.sh:84` | Pretende ser hook: `app/core/hooks/pre-push.sh:2`; ativação atual **[nao verificado]** | Inteira `$LABELS`: `app/core/hooks/pre-push.sh:80`, `app/core/hooks/pre-push.sh:83` |
| `app/core/hooks/pre-push.sh:87` | **Não**, mesmo helper: `app/core/hooks/pre-push.sh:54` | `teste_motor`: `app/core/hooks/pre-push.sh:88`; host fixo `juliani_db_test`: `app/config/settings/teste_motor.py:6`; banco de teste final **[nao verificado]** | Mesmo hook; ativação **[nao verificado]** | Recorte `tenants`: `app/core/hooks/pre-push.sh:87` |
| `app/core/esteira_vigia.py:740` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_esiso`: `app/core/esteira_vigia.py:740` | `_isolar`, callback de `passada`: `app/core/esteira_vigia.py:1473` | Recorte FAIL/ERROR de `green.out`, contra `orig/app`: `app/core/esteira_vigia.py:736`, `app/core/esteira_vigia.py:737` |
| `app/core/esteira_vigia.py:1269` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_revert`: `app/core/esteira_vigia.py:1270` | `auto_revert`, pela passada ou CLI: `app/core/esteira_vigia.py:1552`, `app/core/esteira_vigia.py:1579` | Recorte das falhas; fallback no módulo `core.tests.test_esteira_sem_humano`: `app/core/esteira_vigia.py:1259`, `app/core/esteira_vigia.py:1269` |
| `app/core/esteira_vigia.py:1319` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_congelado`, com `CONGELAR_EM`: `app/core/esteira_vigia.py:1319`, `app/core/esteira_vigia.py:1320` | `curar_por_relogio`, passada ou CLI: `app/core/esteira_vigia.py:1554`, `app/core/esteira_vigia.py:1585` | Recorte das falhas registradas: `app/core/esteira_vigia.py:1310` |
| `app/core/esteira_vigia.py:1406` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_esauto`: `app/core/esteira_vigia.py:1406` | `autocura`, pela passada: `app/core/esteira_vigia.py:1500` | Um ID por execução, `[x[0]]`; número de métodos abrangidos depende desse ID **[nao verificado]**: `app/core/esteira_vigia.py:1406` |
| `app/core/integrador_lote.py:373` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_lote`: `app/core/integrador_lote.py:373` | `_suite(ids)`, pelo bisect: `app/core/integrador_lote.py:782`, `app/core/integrador_lote.py:789` | Recorte dos vermelhos: `app/core/integrador_lote.py:776` |
| `app/core/integrador_lote.py:375` | **Sim**, via régua: `bin/regua.sh:165` | CI/default, pela régua: `bin/regua.sh:159`, `bin/regua.sh:169` | `_suite()` sem IDs, por `rodar`: `app/core/integrador_lote.py:639`, `app/core/integrador_lote.py:672`, `app/core/integrador_lote.py:691` | Inteira: `app/core/integrador_lote.py:368`, `bin/regua.sh:23` |
| `app/core/integrador_lote.py:403` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_lote_iso`: `app/core/integrador_lote.py:403` | `_isolamento`, por `rodar`: `app/core/integrador_lote.py:658` | Recorte dos vermelhos, sem as fatias: `app/core/integrador_lote.py:399` |
| `app/core/integrador_lote.py:769` | **Não**, executor direto: `app/core/esteira_vigia.py:760` | CI; `test_juliani_quarentena`, `SO_QUARENTENA=1`: `app/core/integrador_lote.py:769` | `_conferir_quarentena`, por `_subir`: `app/core/integrador_lote.py:757` | Recorte da quarentena: `app/core/integrador_lote.py:766`; seleção só dos quarentenados: `app/core/runner_quarentena.py:78` |

A cópia de hook em `app/` tem uma declaração de que seria a fonte, mas outro selo registra que essa migração “NAO LANDOU”; o instalador atual produz delegação para **`bin/pre-push.sh`** (`app/core/hooks/hooks_fonte.py:10`, `bin/tests/test_recursos_sourced.sh:28`, `bin/hooks.sh:56`). Portanto, sua presença é comprovada; seu uso atual permanece **[nao verificado]**.

**Delegadores que também disparam essas suites:**

| Arquivo:linha do disparo | Trava, banco e escopo | Quem chama |
|---|---|---|
| `bin/molde_fatia/cadeia.sh:51`, `bin/molde_fatia/cadeia.sh:70`, `bin/molde_fatia/cadeia.sh:81` | **Sim**, via régua; CI/default, inteira: `bin/regua.sh:165`, `bin/regua.sh:168`, `bin/regua.sh:169` | Chamador atual **[nao verificado]**; a esteira atual entrega ao integrador: `bin/molde_fatia/esteira.sh:26` |
| `bin/molde_autocura/cadeia.sh:48`, `bin/molde_autocura/cadeia.sh:67`, `bin/molde_autocura/cadeia.sh:78` | **Sim**, via régua; CI/default, inteira: `bin/regua.sh:165`, `bin/regua.sh:168`, `bin/regua.sh:169` | `bin/molde_autocura/esteira.sh:23` |
| `bin/molde_fatia/esteira.sh:15` | **Não** para MAPA; **sim** para RED/GREEN; bancos e recortes do molde: `bin/molde_fatia/rodar.sh:29`, `bin/molde_fatia/rodar.sh:30`, `bin/molde_fatia/rodar.sh:40`, `bin/molde_fatia/rodar.sh:46`, `bin/molde_fatia/rodar.sh:47` | Lançadores genéricos abaixo; associação com pacote materializado **[nao verificado]** |
| `bin/molde_autocura/esteira.sh:14` | **Não**, RED recortado e GREEN inteiro, nos bancos do molde: `bin/molde_autocura/rodar.sh:20`, `bin/molde_autocura/rodar.sh:24`, `bin/molde_autocura/rodar.sh:26` | Lançadores genéricos abaixo; associação com pacote materializado **[nao verificado]** |
| `bin/integrador.sh:52` | **Sim** para inteira via régua; **não** para recortes do executor Python: `app/core/integrador_lote.py:375`, `bin/regua.sh:165`, `app/core/integrador_lote.py:373`, `app/core/esteira_vigia.py:760`; bancos conforme quadro anterior | Service: `bin/hasner-integrador.service:13`; timer `*:0/5`: `bin/hasner-integrador.timer:6` |
| `bin/vigia_esteira.sh:11` | **Não** para diagnósticos Python; bancos/recortes acima: `app/core/esteira_vigia.py:740`, `app/core/esteira_vigia.py:1269`, `app/core/esteira_vigia.py:1319`, `app/core/esteira_vigia.py:1406`, `app/core/esteira_vigia.py:760` | Service: `bin/hasner-esteira.service:15`; timer `*:0/5`: `bin/hasner-esteira.timer:7`; declarado fora do cron: `app/config/crons.py:739` |
| `app/core/esteira_vigia.py:1141` | **Sim**, via `vigia_arvore`; CI, inteira e possíveis repetições recortadas: `bin/vigia_arvore.sh:83`, `bin/vigia_arvore.sh:88`, `bin/vigia_arvore.sh:106`, `bin/vigia_arvore.sh:131` | `_revigiar`, descrito em `app/core/esteira_vigia.py:1135` |
| `bin/refabricar.sh:59` | Executa `rodar.sh` materializado; **sim/não, banco e escopo [nao verificado]**, dependem desse arquivo | Uso humano documentado: `bin/refabricar.sh:11` |
| `bin/deploy_agendado.sh:73` | Executa `esteira.sh` fornecida; **sim/não, banco e escopo [nao verificado]**, dependem do pacote | Cron avulso gerado em `/etc/cron.d`, fora de `config/crons.py`: `bin/deploy_agendado.sh:54`, `bin/deploy_agendado.sh:55` |
| `bin/fabricante.sh:187` | Lança `esteira.sh` gerada; **sim/não, banco e escopo [nao verificado]**, dependem do pacote | Service: `bin/hasner-fabricante.service:14`; timer: `bin/hasner-fabricante.timer:8` |
| `app/core/esteira_vigia.py:913`, `app/core/esteira_vigia.py:917` | Lança `esteira.sh` por systemd ou fallback; **sim/não, banco e escopo [nao verificado]**, dependem do pacote | `relancar`: `app/core/esteira_vigia.py:936`; passada: `app/core/esteira_vigia.py:1516` |

**(1) O que o selo detecta**

O critério executado é textual e por **arquivo**:

- Varre somente `bin/*.sh`; pula caminhos contendo `test_` e a própria trava (`bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:41`).
- Remove comentários de linha inteira e linhas contendo `pgrep|pkill|pidof`; exige o literal `manage.py test` (`bin/tests/test_trava_teste.sh:53`, `bin/tests/test_trava_teste.sh:54`).
- Havendo `REGUA_DB` em qualquer parte do código restante, isenta o arquivo, salvo se também encontrar `test api chamados` ou o literal `test ${ONLY:-$LABELS}` (`bin/tests/test_trava_teste.sh:59`, `bin/tests/test_trava_teste.sh:60`).
- Para aceitar um arquivo cobrado, basta encontrar `trava_teste.sh`; não verifica a ligação entre essa ocorrência e cada disparo (`bin/tests/test_trava_teste.sh:62`).
- O molde de fatia recebe apenas um `grep` separado procurando a string da trava no arquivo (`bin/tests/test_trava_teste.sh:70`).

| Chamadores do censo | Detecta/cobra? Por quê |
|---|---|
| `suite.sh`, `regua.sh` | **Sim, por arquivo**: usam o padrão especial `${ONLY:-$LABELS}`; a presença da trava satisfaz a checagem (`bin/suite.sh:96`, `bin/suite.sh:102`, `bin/regua.sh:165`, `bin/regua.sh:168`, `bin/tests/test_trava_teste.sh:59`, `bin/tests/test_trava_teste.sh:62`). |
| Ambos os disparos de `bin/pre-push.sh` | **Sim, por arquivo**, sem distinguir os dois comandos; aceita a ocorrência do helper com trava (`bin/pre-push.sh:142`, `bin/pre-push.sh:186`, `bin/pre-push.sh:198`, `bin/tests/test_trava_teste.sh:62`). |
| `regua_relogio.sh` | **Sim**: literal presente, sem `REGUA_DB` explícito; aceita a trava (`bin/regua_relogio.sh:51`, `bin/regua_relogio.sh:55`, `bin/tests/test_trava_teste.sh:54`, `bin/tests/test_trava_teste.sh:60`). |
| Disparos de `vigia_arvore.sh` | **Não cobra**: têm `REGUA_DB` e usam `$1`/`$IDS2`, fora dos padrões especiais, embora realmente passem pela trava (`bin/vigia_arvore.sh:84`, `bin/vigia_arvore.sh:86`, `bin/vigia_arvore.sh:132`, `bin/vigia_arvore.sh:134`, `bin/tests/test_trava_teste.sh:59`). |
| `isolamento.sh`, `placar_code.sh`, `regua_calendario.sh` | **Não cobra**: isenção textual por `REGUA_DB`; `$LABELS` do calendário não casa com o padrão especial (`bin/isolamento.sh:31`, `bin/placar_code.sh:123`, `bin/regua_calendario.sh:44`, `bin/regua_calendario.sh:45`, `bin/tests/test_trava_teste.sh:59`). |
| `molde_fatia/rodar.sh:41` | **Só verifica presença textual**, pelo grep específico (`bin/molde_fatia/rodar.sh:40`, `bin/tests/test_trava_teste.sh:70`). |
| `molde_fatia/rodar.sh:30` | **Não detecta esse bypass**: a trava da outra função satisfaz o mesmo grep (`bin/molde_fatia/rodar.sh:30`, `bin/molde_fatia/rodar.sh:40`, `bin/tests/test_trava_teste.sh:70`). |
| `molde_autocura/rodar.sh` | **Não**: subdiretório fora do glob e sem checagem específica (`bin/molde_autocura/rodar.sh:20`, `bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:70`). |
| Hook em `app/`, executor Python e todos os seus consumidores | **Não**: fora de `bin/*.sh`; o selo não segue chamadas transitivas (`app/core/hooks/pre-push.sh:82`, `app/core/esteira_vigia.py:760`, `bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:54`). |
| Demais delegadores do quadro anterior | **Não os descobre como disparadores de testes**: chamar régua, Python ou esteira não satisfaz o literal exigido (`bin/molde_fatia/cadeia.sh:51`, `bin/integrador.sh:52`, `bin/vigia_esteira.sh:11`, `bin/tests/test_trava_teste.sh:54`). |

O selo também testa serialização e saída 75, mas em **cópia com lock isolado**, não no lock global nem exercitando cada chamador (`bin/tests/test_trava_teste.sh:103`, `bin/tests/test_trava_teste.sh:119`, `bin/tests/test_trava_teste.sh:134`). Não o executei: ele cria arquivos temporários e cópias (`bin/tests/test_trava_teste.sh:77`, `bin/tests/test_trava_teste.sh:101`, `bin/tests/test_trava_teste.sh:103`).

**(2) Motivos declarados para ficar fora**

- **Isolamento, placar e calendário:** há declaração nominal de isenção por banco próprio e por supostamente não executarem `LABELS` inteiro; o relato também declara que ficaram intocados por escopo (`app/docs/RELATO-ARQUIVO.md:38181`). **A justificativa do calendário diverge do código**, que lê `LABELS` e executa tudo três vezes (`bin/regua_calendario.sh:22`, `bin/regua_calendario.sh:41`, `bin/regua_calendario.sh:45`).
- **Isolamento:** declara banco próprio e finalidade de repetir apenas falhas sem a fatia (`bin/isolamento.sh:4`, `bin/isolamento.sh:23`). **Calendário:** declara cópia e banco próprio; o shim, contudo, herda CI, incluindo a regra de host (`bin/regua_calendario.sh:14`, `bin/regua_calendario.sh:30`, `app/config/settings/ci.py:12`).
- **MAPA do molde de fatia:** declara por que executa o selo antes de copiar para a árvore, e usa banco próprio; dispensa individual da trava **[nao verificado]** (`bin/molde_fatia/rodar.sh:23`, `bin/molde_fatia/rodar.sh:29`).
- **Autocura, inclusive seu GREEN inteiro:** bancos próprios estão declarados; motivo específico para dispensar a trava **[nao verificado]** (`bin/molde_autocura/rodar.sh:24`, `bin/molde_autocura/rodar.sh:26`).
- **Diagnósticos Python do vigia e integrador:** declaram isolamento, confirmação com relógio congelado, bisect e conferência informativa; justificativa individual de bypass **[nao verificado]** (`app/core/esteira_vigia.py:726`, `app/core/esteira_vigia.py:1306`, `app/core/integrador_lote.py:776`, `app/core/integrador_lote.py:762`). Existe apenas a isenção geral do selo para recortes com banco próprio (`bin/tests/test_trava_teste.sh:55`, `bin/tests/test_trava_teste.sh:60`).
- **Hook em `app/`:** declara serialização por **outro lock**, `/tmp/regua.lock`, inclusive herança; não comprova dispensa de `trava_teste.sh` (`app/core/hooks/pre-push.sh:29`, `app/core/hooks/pre-push.sh:32`, `app/core/hooks/pre-push.sh:38`). Motivo específico **[nao verificado]**.
- Autorização específica adicional em `CORTES.md` ou `PISTA.md` **[nao verificado]**. Há declaração contrária, “uma suite por vez na trava”, em `app/docs/CORTES.md:258`; o comando canônico mantém até `--only` dentro dela (`CLAUDE.md:174`).

**(3) `bin/trava_teste.sh --quem`**

Ele lê exclusivamente **`/tmp/juliani_db_test.lock.quem`**: se o arquivo não estiver preenchido, ou a leitura falhar, imprime `livre` e termina **antes de consultar o flock** (`bin/trava_teste.sh:21`, `bin/trava_teste.sh:28`, `bin/trava_teste.sh:35`).

O lock real é o flock do descritor 9 sobre **`/tmp/juliani_db_test.lock`**. Depois de adquiri-lo, o script escreve nome informado por `ESTEIRA_QUEM` ou basename, PID do wrapper e horário; o `trap EXIT` escreve `livr

[truncado em 20000 de 21024 caracteres]

## 31/10 16:05 · sessao 05d1cebb902a · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 16:05 · sessao 31daa219b7c3 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 16:05 · sessao e593285a4f13 · - · - · origem=Ronald (tmux)

O selo faz varredura textual, por arquivo, não por disparo: percorre somente `bin/*.sh`, pula caminhos contendo `test_` e `trava_teste.sh`, remove comentários de linha inteira e linhas contendo `pgrep|pkill|pidof`, exige literal `manage.py test`, e aceita a presença textual de `trava_teste.sh`. (`bin/tests/test_trava_teste.sh:40-63`)

A isenção efetiva é: se o texto contém `REGUA_DB`, pula o arquivo, **exceto** se também contiver `test api chamados` ou o literal `test ${ONLY:-$LABELS}`. Não avalia o valor do banco nem segue variáveis, funções ou cadeias de scripts. (`bin/tests/test_trava_teste.sh:59-62`)

Aplicação aos disparadores conhecidos:

| Chamador | Selo detecta/cobra? | Prova |
|---|---|---|
| `bin/pre-push.sh` — ambos os disparos | Sim, por arquivo; aceita string da trava | `bin/pre-push.sh:142`, `:186`, `:198`; critério `bin/tests/test_trava_teste.sh:54-62` |
| `bin/regua.sh` | Sim; padrão especial impede isenção por `REGUA_DB` | `bin/regua.sh:165-168`; `bin/tests/test_trava_teste.sh:59-62` |
| `bin/suite.sh` | Sim; mesmo padrão especial | `bin/suite.sh:96-102`; `bin/tests/test_trava_teste.sh:59-62` |
| `bin/regua_relogio.sh` | Sim; não passa `REGUA_DB` | `bin/regua_relogio.sh:49-55`; `bin/tests/test_trava_teste.sh:54-62` |
| `bin/vigia_arvore.sh` — ambos os locais | **Não cobra**: encontra literal, mas isenta por `REGUA_DB`; os comandos usam `$1` e `$IDS2`, não os padrões especiais | `bin/vigia_arvore.sh:84-86`, `:132-134`; `bin/tests/test_trava_teste.sh:59-61` |
| `bin/isolamento.sh` | Não cobra: isento por `REGUA_DB` | `bin/isolamento.sh:31-33`; `bin/tests/test_trava_teste.sh:59-61` |
| `bin/placar_code.sh` | Não cobra: isento por `REGUA_DB` | `bin/placar_code.sh:123-126`; `bin/tests/test_trava_teste.sh:59-61` |
| `bin/regua_calendario.sh` | **Não cobra, embora rode LABELS inteiro**: isenção textual por `REGUA_DB`, comando usa `$LABELS` | `bin/regua_calendario.sh:22`, `:44-45`; `bin/tests/test_trava_teste.sh:59-61` |
| `bin/molde_fatia/rodar.sh:41` | Só grep separado procurando string da trava no arquivo bruto | `bin/tests/test_trava_teste.sh:70-71`; trava presente em `bin/molde_fatia/rodar.sh:40` |
| `bin/molde_fatia/rodar.sh:30` | **Não detecta o bypass deste disparo**: grep anterior aceita a trava presente na outra função | `bin/molde_fatia/rodar.sh:28-30`, `:40-41`; `bin/tests/test_trava_teste.sh:70-71` |
| `bin/molde_autocura/rodar.sh` | Não: subdiretório fora do glob, sem verificação específica | disparo `bin/molde_autocura/rodar.sh:20`; glob/verificação específica `bin/tests/test_trava_teste.sh:40`, `:70` |
| `app/core/hooks/pre-push.sh` — ambos | Não: fora de `bin/*.sh` | `app/core/hooks/pre-push.sh:82`, `:87`; `bin/tests/test_trava_teste.sh:40` |
| `app/core/esteira_vigia.py::_rodar_testes` | Não: fora de `bin/*.sh` | `app/core/esteira_vigia.py:751-760`; `bin/tests/test_trava_teste.sh:40` |
| Wrappers que só chamam `suite.sh`/`regua.sh` | Não descobertos diretamente se não contêm `manage.py test`; nenhuma análise transitiva | `bin/tests/test_trava_teste.sh:54`, `:62` |

Além disso, cobra executável da trava, pelo menos dois arquivos aceitos, e testa serialização/rc 75 usando **cópia com lock isolado**; não exercita o lock global nem prova cobertura de todos os chamadores. (`bin/tests/test_trava_teste.sh:20`, `:85`, `:103-110`, `:119-137`)

Motivos declarados:

- Isenção genérica de recortes com banco próprio está escrita no selo: `bin/tests/test_trava_teste.sh:8`, `:27-28`, `:55-61`.
- Documento nomeia `isolamento.sh`, `placar_code.sh` e `regua_calendario.sh` como sem trava, com `REGUA_DB` próprio, “isenção declarada do selo, porque nao rodam o LABELS inteiro”; também declara não mexer por escopo da obra. **A parte referente ao calendário diverge do código atual**, que deriva LABELS e roda a lista completa três vezes. (`app/docs/RELATO-ARQUIVO.md:38181`; `bin/regua_calendario.sh:22`, `:41-45`)
- `app/core/hooks/pre-push.sh` declara serialização por **outro lock**, `/tmp/regua.lock`, herdável com `REGUA_TRAVA_HERDADA=1`; não declara autorização específica para ficar fora de `trava_teste.sh`. (`app/core/hooks/pre-push.sh:29-41`; motivo específico `[nao verificado]`)
- Para `molde_autocura`, o disparo isolado do MAPA em `molde_fatia` e `_rodar_testes`, motivo explícito individual para bypass `[nao verificado]`; os bancos próprios estão no código, mas isso sozinho não declara uma exceção individual. (`bin/molde_autocura/rodar.sh:20`, `:24-26`; `bin/molde_fatia/rodar.sh:28-30`; `app/core/esteira_vigia.py:752`)
- A regra canônica manda `--only` passar pela trava e diz que `regua.sh`/`bin/pre-push.sh` mantêm chamadas próprias **porque já pegam a trava**, evitando aninhamento. (`CLAUDE.md:174-179`)
- Não localizei autorização específica de bypass em `app/docs/CORTES.md` ou `app/docs/PISTA.md` `[nao verificado]`; há, em sentido contrário, “uma suite por vez na trava” e “um push só, sem outra suite rodando junto”. (`app/docs/CORTES.md:258`, `:349`)

`--quem`:

- Lê exclusivamente `/tmp/juliani_db_test.lock.quem`; se não estiver não vazio, ou o `cat` falhar, imprime `livre` e sai antes de abrir/consultar `flock`. (`bin/trava_teste.sh:21`, `:28`, `:35-36`)
- O lock real é adquirido no descritor 9 sobre `/tmp/juliani_db_test.lock`; só depois o script escreve o informativo. O texto contém `ESTEIRA_QUEM` ou basename, PID do wrapper e data; não identidade conferida de container ou processo Django. (`bin/trava_teste.sh:23`, `:35-40`)
- Pode ler informação anterior entre aquisição e escrita, ou `livre desde...` durante o trap antes de encerrar o wrapper: informativo e lock não são uma leitura atômica. (`bin/trava_teste.sh:35-42`, inferência da ordem)
- Se o wrapper morrer sem executar o trap, o último detentor pode permanecer no arquivo; ocorrência real nesta árvore `[nao verificado]`. A atualização para `livre` depende do trap EXIT. (`bin/trava_teste.sh:40-41`)
- Suites sem a trava são invisíveis ao informativo; a própria documentação declara que `livre` nunca garante ausência de suite. (`CLAUDE.md:203-205`; `bin/suite.sh:7-13`)
- Há relato explícito de containers órfãos ainda testando enquanto `--quem` respondia `livre` após morte do lançador; a existência deles agora `[nao verificado]`. (`app/docs/BACKLOG.md:1421`; `app/docs/RELATO-ARQUIVO.md:1380`)


## 31/10 15:53 · sessao 099205dd302f · TRAVA-MENOS-FILA · leitura · origem=Code

**(a) O histórico deve nascer dentro de `bin/trava_teste.sh`, em torno da aquisição e da execução.** Instrumentar apenas `suite.sh` perderia régua, pre-push e demais entradas diretas (`bin/regua.sh:165`, `bin/pre-push.sh:142`, `bin/vigia_arvore.sh:83`).

Os pontos são:

- **Pedido:** depois de validar argumentos e imediatamente antes de abrir/esperar o lock; nunca depois do `flock`, pois apagaria a espera (`bin/trava_teste.sh:33`, `bin/trava_teste.sh:35-36`).
- **Início:** imediatamente após o `flock` conseguir a vez, antes de escrever `.quem` e executar o comando. Assim mede espera pela trava, separada do trabalho protegido (`bin/trava_teste.sh:36-42`).
- **Fim e rc:** após `"$@"`, capturando `$?` antes de qualquer escrita. Hoje esse comando é a última instrução: acrescentar um append sem preservar o status pode trocar o resultado do teste pelo resultado do histórico (`bin/trava_teste.sh:42`).
- **Sem vez:** registrar tentativa encerrada com `inicio=null`, fim e `rc=75`, preservando a saída atual. O finalizador precisa cobrir esse caminho e conservar a limpeza de `.quem` somente quando houve aquisição; hoje o `EXIT` é instalado depois do caminho que sai 75 (`bin/trava_teste.sh:36-41`).
- **Lock do histórico:** usar outro descritor, em escopo curto e com espera limitada; substituir o FD 9 ou manter o lock do histórico durante o run alteraria o dispositivo atual. O FD 9 pertence à trava principal (`bin/trava_teste.sh:35-42`).

**Uma linha final por tentativa serve para histórico concluído; não serve, sozinha, para publicar a fila corrente.** Pedido, início e término precisam de eventos correlacionados ou de estado corrente separado: antes da aquisição ainda não existe início, e antes de terminar não existem fim/rc (`bin/trava_teste.sh:36-42`).

**(b) Chamadores diretos encontrados e identificação disponível:**

| Chamador | O que transmite à trava |
|---|---|
| Régua | `ESTEIRA_QUEM=regua` (`bin/regua.sh:165`). |
| Pre-push | `pre-push`; a função atende arsenal e control-plane, portanto um push pode produzir duas tentativas (`bin/pre-push.sh:142`, `bin/pre-push.sh:186`, `bin/pre-push.sh:198`). |
| `suite.sh` | Identidade herdada ou `suite` (`bin/suite.sh:96`). |
| Vigia da árvore | `vigia_arvore`, em dois pontos de execução (`bin/vigia_arvore.sh:83`, `bin/vigia_arvore.sh:131`). |
| Régua do relógio | `regua_relogio` (`bin/regua_relogio.sh:51`). |
| Molde das fatias | `fatia`, nos RED/GREEN parciais (`bin/molde_fatia/rodar.sh:40`, `bin/molde_fatia/rodar.sh:46-47`). |

**Todos têm ferramenta nomeável; não têm obra obrigatoriamente identificada.** A trava constrói `quem` como ferramenta mais PID; o molde conhece `__FATIA__`, mas transmite somente `fatia` (`bin/trava_teste.sh:23`, `bin/molde_fatia/rodar.sh:40`, `bin/molde_fatia/rodar.sh:46`).

O “label de `suite.sh`” não resolve: `--only` e `LABELS` são alvos de testes, não identificadores da obra; o parser não oferece argumento de obra (`bin/suite.sh:43-53`, `bin/suite.sh:59`, `bin/suite.sh:102`). Uso padronizado de `SUITE_OBRA` pelos chamadores: [nao verificado].

O instalador de hooks gera delegação a `bin/pre-push.sh`; há também uma cópia em `app/core/hooks/pre-push.sh` que executa Docker diretamente. Qual hook está efetivamente instalado: [nao verificado] (`bin/hooks.sh:51-56`, `app/core/hooks/pre-push.sh:54`, `app/core/hooks/pre-push.sh:82`, `app/core/hooks/pre-push.sh:87`).

**(c) O contrato precisa corrigir estes pontos:**

1. **“Único portão do `juliani_db_test`” excede a cobertura existente.** Calendário e molde de autocura executam suites completas diretamente; isolamento, placar, selo da fatia e vigia Python também executam testes sem passar pela trava (`bin/regua_calendario.sh:41-45`, `bin/molde_autocura/rodar.sh:20-26`, `bin/isolamento.sh:31-33`, `bin/placar_code.sh:122-126`, `bin/molde_fatia/rodar.sh:28-30`, `app/core/esteira_vigia.py:751-760`). `REGUA_DB` muda o banco de teste, enquanto o host padrão continua `juliani_db_test` (`app/config/settings/ci.py:12`, `app/config/settings/ci.py:17`). O selo não garante universalidade: varre `bin/*.sh`, tem exceções por `REGUA_DB` e aceita presença textual da trava no arquivo (`bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:59-62`).

2. **“Pedidos sem início” inclui tentativas já encerradas.** Um timeout sai sem executar o comando; contá-lo como pendente produziria fila permanente. É necessário distinguir aguardando, admitido e encerrado sem admissão (`bin/trava_teste.sh:36-42`). Além disso, `rc=75` isolado não prova timeout: o comando protegido também pode devolver 75; o registro precisa indicar se houve aquisição (`bin/trava_teste.sh:38`, `bin/trava_teste.sh:42`).

3. **`--quem` não prova posse nem mede fila.** Ele lê apenas um arquivo lateral; as atualizações acontecem depois da aquisição e no `EXIT`, permitindo divergência entre texto e lock, inclusive após morte abrupta (`bin/trava_teste.sh:28`, `bin/trava_teste.sh:40-41`).

4. **Continuar o run após append falho é coerente com “hist é testemunha”, mas deixa cobertura incompleta.** “Conta FALHA” precisa de canal independente: tentar registrar a falha somente no histórico que falhou pode torná-la invisível. Esse canal não existe na trava atual (`bin/trava_teste.sh:20-42`). O contrato também precisa impedir espera indefinida pelo lock do histórico; o mecanismo proposto ainda está [nao verificado].

5. **A média pode esconder justamente as piores esperas.** Excluir tentativas sem início retira os timeouts; atribuir-lhes zero também falseia a espera. Publicaria média dos admitidos com quantidade de amostras, negados separados e idade dos pendentes, pois o caminho 75 efetivamente esperou e não começou (`bin/trava_teste.sh:36-38`). Janela por pedido/início/fim, fuso, dias civis versus janelas móveis, gatilho por obra versus agregado e fonte da conta da VM não estão definidos no contrato enviado: [nao verificado].

6. **O destino do ESTADO precisa ser corrigido.** O fluxo encontrado gera `$TMP/ESTADO.md` e publica no destino padrão `/home/ronald/hasner-relato`, não em `app/docs/ESTADO.md` (`bin/relato.sh:23`, `bin/relato.sh:38`, `bin/relato.sh:100-102`, `bin/gerar_estado.py:376`). O template atual não contém bloco TRAVA (`bin/gerar_estado.py:295-369`). O novo leitor deve conter falhas de importação, leitura e composição para publicar `nao sei`; sem contenção, erro do gerador encerra o ciclo de publicação (`bin/gerar_estado.py:289-293`, `bin/relato.sh:38`).

7. **A instrumentação precisa preservar o isolamento dos testes da própria trava.** O selo copia o script e altera apenas o caminho do lock; acrescentar histórico com caminho real pode fazer suas execuções sintéticas contaminarem a medida publicada (`bin/tests/test_trava_teste.sh:101-103`, `bin/tests/test_trava_teste.sh:119-137`).

## 31/10 15:53 · sessao 1d030a5d304b · V3-RETOMA · leitura · origem=Code

**Os textos examinados usam a duração diária para as extras e a faixa horária para o trabalho noturno. Não estabelecem expressamente que as extras sejam sempre os últimos minutos trabalhados, nem disciplinam sua distribuição entre vários períodos intradiários.** Essa constatação limita-se aos dispositivos e verbetes abaixo. [CLT, normaalizada-pe.html:300–307, 360–368](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html)

As citações entre aspas são **trechos literais selecionados**; as explicações restantes são paráfrases. As linhas indicam a representação textual dos documentos externos consultados.

**1. CLT: dispositivo, literalidade e situação**

O art. 59, caput e §§ 1º, 2º, 3º, 5º e 6º, consta do texto atualizado; o § 4º está revogado. [normaatualizada-pe.html:300–307](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html)

| Dispositivo | Trecho literal selecionado | Conteúdo, em paráfrase |
|---|---|---|
| Art. 59, caput | “A duração diária do trabalho poderá ser acrescida de horas extras” | Refere-se à duração **diária**; prevê até duas extras mediante os instrumentos indicados. [l13467.htm:105](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |
| § 1º | “50% (cinquenta por cento)” | Acréscimo mínimo sobre a hora normal. [l13467.htm:107](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |
| § 2º | “em um dia” | Permite compensar excesso diário pela diminuição em outro dia, mediante acordo ou convenção coletiva, observados o período máximo anual e o limite de dez horas diárias. [normaatualizada-pe.html:302](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html) |
| § 3º | “horas extras não compensadas” | Determina seu pagamento na rescisão, segundo a remuneração dessa data. [l13467.htm:111](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |
| § 4º | — | **Revogado** pela Lei 13.467/2017. [l13467.htm:113, 987–992](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |
| § 5º | “seis meses” | Banco de horas por acordo individual escrito, com compensação nesse prazo máximo. [l13467.htm:115](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |
| § 6º | “no mesmo mês” | Compensação por acordo individual tácito ou escrito. [l13467.htm:117](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |

Os quatro parágrafos solicitados do art. 73 constam do texto atualizado, sem indicação de revogação. [normaatualizada-pe.html:361–368](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html)

| Dispositivo | Trecho literal selecionado | Conteúdo, em paráfrase |
|---|---|---|
| Art. 73, § 1º | “30 (trinta) segundos” | Hora noturna computada com 52 minutos e 30 segundos. [normaatualizada-pe.html:361](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html) |
| § 2º | “Considera-se noturno” | Define o trabalho entre 22h de um dia e 5h do seguinte. [normaatualizada-pe.html:363](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html) |
| § 4º | “Nos horários mistos” | Aplica as disposições do artigo às horas noturnas dos horários que abrangem períodos diurnos e noturnos. [normaatualizada-pe.html:366](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html) |
| § 5º — íntegra | “Às prorrogações do trabalho noturno aplica-se o disposto neste capítulo.” | Estende as disposições do capítulo à prorrogação. [normaatualizada-pe.html:368](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html) |

Portanto, **quantificação diária**, **localização no relógio** e **prorrogação noturna** aparecem nesses dispositivos; uma regra expressa de seleção cronológica dos minutos extraordinários não aparece. [normaatualizada-pe.html:300–307, 360–368](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html)

**2. Súmulas e OJs indicadas**

| Verbete | Texto literal relevante | O que o trecho efetivamente disciplina |
|---|---|---|
| **Súmula 60, II** | “Cumprida integralmente a jornada no período noturno e prorrogada esta, devido é também o adicional quanto às horas prorrogadas.” | Adicional na prorrogação noturna. Transcrição do verbete em documento oficial: [acordao.do, E-ED-RR-825-88.2010.5.03.0064:171–176](https://consultadocumento.tst.jus.br/consultaDocumento/acordao.do?anoProcInt=2013&dtaPublicacaoStr=28%2F05%2F2021+07%3A00%3A00&nia=7637301&numProcInt=170108). |
| **Súmula 85, III e IV** | “horas excedentes à jornada normal diária” | Compensação e consequências de sua invalidade, com referências aos módulos diário e semanal. [documento d4a380524f995d9b0bb461bf24733a13:89–91](https://jurisprudencia-backend.tst.jus.br/rest/documentos/d4a380524f995d9b0bb461bf24733a13). |
| **Súmula 366** | “Se ultrapassado esse limite, será considerada como extra a totalidade do tempo que exceder a jornada normal.” | Variações de registro e tolerância; o trecho não identifica necessariamente o excesso com o final do dia. [documento 8e88a25e9f40b1e5e2199ac0e3dd8c43:147–149](https://jurisprudencia-backend.tst.jus.br/rest/documentos/8e88a25e9f40b1e5e2199ac0e3dd8c43). |
| **Súmula 437, I** | “sem prejuízo do cômputo da efetiva jornada de labor para efeito de remuneração” | Pagamento relacionado à supressão do intervalo e preservação do cômputo do trabalho efetivo. [documento f22a2b6f90b469ac17ad6ba9b3c9e847:51–54](https://jurisprudencia-backend.tst.jus.br/rest/documentos/f22a2b6f90b469ac17ad6ba9b3c9e847). |
| **OJ SDI-1 97 — íntegra** | “O adicional noturno integra a base de cálculo das horas extras prestadas no período noturno.” | **Base de cálculo** das extras noturnas. [JURISPRUDÊNCIA CONSOLIDADA_VERSÃO_COMPLETA_28_6_2024.pdf:8318–8320](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1). |
| **OJ SDI-1 388** | “após as 5 horas da manhã” | Adicional nas horas posteriores às 5h, em jornada 12×36 que compreenda todo o período noturno. [JURISPRUDÊNCIA CONSOLIDADA_VERSÃO_COMPLETA_28_6_2024.pdf:10520–10528](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1). |

**Situação dos verbetes:**

- **366 e 437: canceladas.** A Resolução 225/2025 registra, respectivamente nos incisos XV e XXII, cancelamento por perda de eficácia desde 11/11/2017 pela Lei 13.467/2017. Trecho literal comum: “cancelada por perda de eficácia a partir de 11/11/2017”. [2025_res0225.pdf:83–84, 99–100](https://juslaboris.tst.jus.br/bitstream/handle/20.500.12178/251919/2025_res0225.pdf?isAllowed=y&sequence=1)
- **60, 85, OJ 97 e OJ 388:** constam sem cancelamento na edição oficial de 2024 e não figuram nos cancelamentos da Resolução 225/2025. **Vigência atual em 31/10/2026: [NAO VERIFICADO].** [JURISPRUDÊNCIA CONSOLIDADA_VERSÃO_COMPLETA_28_6_2024.pdf:3007–3014, 3212–3236, 8318–8320, 10520–10528](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1); [2025_res0225.pdf:51–136](https://juslaboris.tst.jus.br/bitstream/handle/20.500.12178/251919/2025_res0225.pdf?isAllowed=y&sequence=1)

**3. Resposta específica sobre quais minutos são extras**

Nos seis verbetes examinados, **não há comando literal que reserve as extras aos últimos minutos cronológicos do dia**, nem regra específica para atribuir extras noturnas quando a prorrogação ocorre em período anterior ao último. [JURISPRUDÊNCIA CONSOLIDADA_VERSÃO_COMPLETA_28_6_2024.pdf:3007–3014, 3212–3236, 6280–6288, 7293–7312, 8318–8320, 10520–10528](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1)

A OJ 97 pressupõe horas extras prestadas em período noturno e determina sua composição remuneratória; ela não fornece o critério anterior para escolher esses minutos. A OJ 388, por sua vez, fala em adicional após as 5h sem qualificar essas horas como extraordinárias. **Essa distinção é leitura delimitada dos textos.** [Mesmo PDF:8320, 10526–10528](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1)

**[NAO VERIFICADO]** Não consegui fundamentar literalmente uma regra geral que imponha: **“extras = últimos minutos trabalhados”**, **“excesso apurado separadamente por período”** ou uma solução específica para o período anterior ao último. Também permanece **[NAO VERIFICADO]** afirmar que inexista qualquer outra Súmula ou OJ do TST sobre essa seleção temporal.

## 31/10 15:52 · sessao 2e56327f1075 · - · - · origem=Ronald (tmux)

O ESTADO publicado nasce em temporário, não em `app/docs/ESTADO.md`: `bin/relato.sh:38` chama o gerador com `$TMP/ESTADO.md`; o gerador escreve o destino recebido em `bin/gerar_estado.py:376`; o publicador envia o temporário ao raspador em `bin/relato.sh:100-102`, com destino padrão `/home/ronald/hasner-relato` (`bin/relato.sh:23`), e o raspador grava em `dest/dst` (`bin/raspar_publico.py:40`). O template atual não contém TRAVA, e suas métricas vêm dos leitores importados em `bin/gerar_estado.py:198-293` e do template em `bin/gerar_estado.py:295-369`.

Lacunas do contrato:

- **Uma linha final por run é insuficiente para fila corrente.** O início só fica conhecido depois de `flock` (`bin/trava_teste.sh:36-40`) e fim/rc depois do comando (`bin/trava_teste.sh:42`); se a linha nascer ao terminar, os pedidos ainda aguardando não aparecem. Se nascer antes, faltam início/fim/rc e são necessárias atualizações ou eventos correlacionados por ID. É consequência do desenho existente; o protocolo adicional está [nao verificado].
- **Pedidos sem início incluem encerrados.** Timeout sai por `75` antes do comando (`bin/trava_teste.sh:36-38`); o contrato precisa distinguir aguardando, negado e cancelado para não transformar timeout em fila eterna. Cancelamento/sinais não têm protocolo atual além do `EXIT` do detentor (`bin/trava_teste.sh:41`).
- **`--quem` é testemunho lateral.** Ele somente lê `$TRAVA.quem` ou imprime `livre` (`bin/trava_teste.sh:28`); o arquivo é atualizado depois da aquisição (`bin/trava_teste.sh:40`) e no `EXIT` (`bin/trava_teste.sh:41`). Logo há janelas de divergência entre o arquivo e o lock; o contrato não deve tratar essa leitura como prova suficiente da posse.
- **A observação não pode segurar a pista indefinidamente.** O lock atual ocupa FD 9 antes da espera e permanece durante o comando (`bin/trava_teste.sh:35-42`); usar FD próprio e espera limitada para o histórico evita substituir FD 9 ou aumentar a ocupação da pista. O prazo e o canal durável de FALHA do histórico estão [nao verificado].
- **Falha do histórico pode fabricar média boa.** Se append falhar e o run prosseguir, os runs omitidos não aparecem na média; “conta FALHA” precisa de canal independente e de indicação de cobertura incompleta. O mecanismo ainda está [nao verificado]; a trava atual não tem histórico nem contador (`bin/trava_teste.sh:20-42`).
- **`nao sei` precisa cobrir toda a seção.** O gerador já protege importação, leitura e composição de outro medidor conjuntamente (`bin/gerar_estado.py:287-293`); sem isso, uma exceção do novo leitor impede a geração e `relato.sh` encerra o ciclo (`bin/relato.sh:38`). Arquivo vazio, registros truncados, falha de permissões e ausência de amostras precisam de semântica explícita [nao verificado].
- **A média tem viés sem universo definido.** Excluir os `75` omite justamente quem esperou e não conseguiu iniciar (`bin/trava_teste.sh:36-38`); incluí-los como espera zero também falseia a medida. Devem aparecer separados como negados, com tempo observado como limite inferior, e pendentes com idade corrente [nao verificado].
- **“24 h” e “2 dias seguidos” são insuficientes para o gatilho.** O contrato não fixa inclusão por pedido/início/fim, fuso, dias civis versus janelas móveis, mínimo de amostras, agregado global versus por obra nem origem da conta da VM [nao verificado]. Datas com ano/fuso e duração monotônica são recomendáveis; o carimbo atual de `--quem` contém somente dia/mês/hora (`bin/trava_teste.sh:40-41`).

Há precedente útil de observabilidade na sombra: ela mede antes/depois do `flock`, registra inclusive negação (`bin/sombra_trava.sh:27-35`) e conta negados e linhas ilegíveis como problemas (`bin/sombra_trava.sh:55-60`), embora esse formato também não registre a fila enquanto espera (`bin/sombra_trava.sh:28-31`).

O selo atual só verifica serialização e timeout `75`, sem história ou medição de fila (`bin/tests/test_trava_teste.sh:117-137`).

## 31/10 15:52 · sessao ee9b1d9ee7f7 · - · - · origem=Ronald (tmux)

Chamadores diretos encontrados, todos com identidade de ferramenta, sem obra obrigatória:

| Chamador | Identidade transmitida | Evidência |
|---|---|---|
| Régua | `ESTEIRA_QUEM=regua` | `bin/regua.sh:165` |
| Pre-push | `ESTEIRA_QUEM=pre-push`, na função compartilhada pelo arsenal e pelo control-plane | `bin/pre-push.sh:142`, `bin/pre-push.sh:186`, `bin/pre-push.sh:198` |
| Suite | `ESTEIRA_QUEM` herdado ou `suite` | `bin/suite.sh:96` |
| Vigia da árvore | `ESTEIRA_QUEM=vigia_arvore`, para suite/reteste e confirmação viva | `bin/vigia_arvore.sh:83`, `bin/vigia_arvore.sh:88`, `bin/vigia_arvore.sh:106`, `bin/vigia_arvore.sh:131` |
| Régua do relógio | `ESTEIRA_QUEM=regua_relogio` | `bin/regua_relogio.sh:51` |
| Molde das fatias | `ESTEIRA_QUEM=fatia`, usado nos RED/GREEN parciais | `bin/molde_fatia/rodar.sh:40`, `bin/molde_fatia/rodar.sh:46`, `bin/molde_fatia/rodar.sh:47` |

`QUEM` acrescenta PID à identidade, portanto identifica uma execução da ferramenta, sem identificar necessariamente a obra (`bin/trava_teste.sh:23`). O molde dispõe de `__FATIA__` e bancos específicos, mas envia apenas `fatia` à trava (`bin/molde_fatia/rodar.sh:29`, `bin/molde_fatia/rodar.sh:40`, `bin/molde_fatia/rodar.sh:46`). O parser de `suite.sh` aceita `--dir`, `--only`, `--parallel`, `--espera` e argumentos extras; não oferece argumento de obra (`bin/suite.sh:43`, `bin/suite.sh:46`, `bin/suite.sh:50`, `bin/suite.sh:52`). `LABELS` é lista de apps de teste lida da régua, utilizada como alvo do `manage.py test`, não identificador de obra (`bin/suite.sh:59`, `bin/suite.sh:102`). Não encontrei uso de `SUITE_OBRA` na busca do repositório [nao verificado].

O hook que o instalador gera delega a `bin/pre-push.sh`; a instalação efetiva nesta máquina não foi conferida (`bin/hooks.sh:51`, `bin/hooks.sh:56`; [nao verificado]). Existe uma cópia antiga em `app/core/hooks/pre-push.sh` que executa Docker diretamente, sem passar por `trava_teste.sh`; não há prova de que esteja instalada (`app/core/hooks/pre-push.sh:54`, `app/core/hooks/pre-push.sh:82`, `app/core/hooks/pre-push.sh:87`; [nao verificado]).

A premissa “único portão do juliani_db_test” não é verdadeira para todo executor existente: o CI aponta por default ao mesmo container e apenas varia o nome do banco quando recebe `REGUA_DB` (`app/config/settings/ci.py:12`, `app/config/settings/ci.py:17`).

- `regua_calendario.sh` executa a suite inteira diretamente três vezes, com bancos próprios no container default (`bin/regua_calendario.sh:41`, `bin/regua_calendario.sh:43`, `bin/regua_calendario.sh:44`, `bin/regua_calendario.sh:45`).
- O molde de autocura executa Docker diretamente e inclui um GREEN completo (`bin/molde_autocura/rodar.sh:20`, `bin/molde_autocura/rodar.sh:25`, `bin/molde_autocura/rodar.sh:26`).
- Há recortes diretos no isolamento, placar e selo do molde de fatia (`bin/isolamento.sh:31`, `bin/isolamento.sh:33`; `bin/placar_code.sh:121`, `bin/placar_code.sh:125`; `bin/molde_fatia/rodar.sh:28`, `bin/molde_fatia/rodar.sh:30`).
- O vigia Python também monta e executa Docker diretamente (`app/core/esteira_vigia.py:751`, `app/core/esteira_vigia.py:757`, `app/core/esteira_vigia.py:760`).

O selo não prova cobertura universal: percorre somente `bin/*.sh`, aceita presença textual de `trava_teste.sh` em qualquer parte do código e isenta `REGUA_DB` fora de dois padrões limitados de suite inteira (`bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:59`, `bin/tests/test_trava_teste.sh:60`, `bin/tests/test_trava_teste.sh:62`). Seus exercícios da trava usam uma cópia isolada, não o dispositivo global; instrumentação nova não deve contaminar o histórico real com esses runs (`bin/tests/test_trava_teste.sh:101`, `bin/tests/test_trava_teste.sh:103`, `bin/tests/test_trava_teste.sh:119`, `bin/tests/test_trava_teste.sh:134`).

## 31/10 15:50 · sessao b06ea5b91449 · - · - · origem=Ronald (tmux)

Não localizei fonte primária que formule literalmente a regra “as horas extras são necessariamente os últimos minutos trabalhados cronologicamente”, nem regra específica de imputação quando o excesso ocorre em período anterior ao último. A **inexistência universal** desse verbete permanece **[NAO VERIFICADO]**; não recomendo transformar o resultado negativo da busca em afirmação de inexistência.

Achados úteis para a consolidação:

- **Súmula 264** trata de composição remuneratória, não de seleção temporal dos minutos. Sua reafirmação no **Tema 280**, processo **RR-0000254-24.2023.5.09.0411**, consta de acórdão primário do TST, datado de Brasília, **25/08/2025**, reproduzido no [PDF oficial hospedado pelo TRT3](https://portal.trt3.jus.br/internet/jurisprudencia/incidentes-suscitados-irr-iac-arginc-tst/downloads/acordao-de-merito-tema-280.pdf), **ID acd3354, pág. 7**. Trecho literal curto: “A remuneração das horas extraordinárias é composta do valor da hora normal”. A numeração das linhas não pôde ser obtida: **[NAO VERIFICADO: arquivo:linha]**. O resultado indexado contém o dispositivo integral.
- **OJ SDI-1 127**: refere-se à subsistência da hora reduzida após a Constituição; não oferece regra de distribuição cronológica. **OJ SDI-1 395**: refere-se à compatibilidade entre hora reduzida e turnos ininterruptos de revezamento; também não seleciona minutos extraordinários. Ambas aparecem no [Livro oficial do TST](https://www.tst.jus.br/documents/10157/63003/Livro-Internet.pdf), em resultados indexados; a OJ395 aparece na página interna **C-97** da edição indexada. Vigência atual e linhas: **[NAO VERIFICADO]**, pois o documento aberto retornou 403.
- A redação da **OJ388** sobre empregado submetido a jornada **12x36** ajuda a distinguir **prorrogação do período noturno** de **excesso da jornada contratual**: o texto trata de adicional após 5h dentro desse regime, sem identificar tais horas como extras. Essa distinção é uma **inferência do texto**, não transcrição de regra geral. O outro agente deve fornecer o texto e a situação confirmados.
- As buscas pelos termos “últimos minutos”, “ordem cronológica”, “primeiro período”, “último período” e “dois períodos”, associados a horas extras/noturnas no domínio TST, não produziram enunciado específico. Essa é uma limitação da pesquisa, não prova jurídica de inexistência. **[NAO VERIFICADO]**.

O endereço aparentemente atual do livro é [livrointernet-12-pdf](https://www.tst.jus.br/documents/d/guest/livrointernet-12-pdf), mas também retornou 403. A rede no shell não resolveu DNS; nenhum arquivo foi criado, editado ou apagado.

## 31/10 15:50 · sessao 85b784e75287 · - · - · origem=Ronald (tmux)

Fontes oficiais acessíveis:

- **Livro do TST reproduzido pelo TRT3**, atualizado em 28/06/2024: [JURISPRUDÊNCIA CONSOLIDADA_VERSÃO_COMPLETA_28_6_2024.pdf](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1). A própria obra informa que incorporou o livro do TST extraído do sítio oficial, linhas146–148. Permite reprodução de qualquer parte com citação da fonte, linhas42–43.
- **Resolução TST225/2025**: [2025_res0225.pdf](https://juslaboris.tst.jus.br/bitstream/handle/20.500.12178/251919/2025_res0225.pdf?isAllowed=y&sequence=1),141 linhas.

**Súmula60,II**, livro TRT3:3012–3014,página73:

> Cumprida integralmente a jornada no período noturno e prorrogada esta, devido é também o adicional quanto às horas prorrogadas. Exegese do art.73,§5º,da CLT.

ItemI,livro TRT3:3010–3011:

> O adicional noturno, pago com habitualidade, integra o salário do empregado para todos os efeitos.

**Súmula85,III**,livro TRT3:3221–3225,página77:

> O mero não atendimento das exigências legais para a compensação de jornada, inclusive quando encetada mediante acordo tácito, não implica a repetição do pagamento das horas excedentes à jornada normal diária, se não dilatada a jornada máxima semanal, sendo devido apenas o respectivo adicional.

ItemIV,livro TRT3:3226–3230:

> A prestação de horas extras habituais descaracteriza o acordo de compensação de jornada. Nesta hipótese, as horas que ultrapassarem a jornada semanal normal deverão ser pagas como horas extraordinárias e, quanto àquelas destinadas à compensação, deverá ser pago a mais apenas o adicional por trabalho extraordinário.

Inteiro teor dos itensI–VI:3212–3236. Não contém regra que selecione cronologicamente os últimos minutos como extras; trata de compensação de jornada, conforme esse trecho delimitado.

**Súmula366**,livro TRT3:6280–6288,página129:

> Não serão descontadas nem computadas como jornada extraordinária as variações de horário do registro de ponto não excedentes de cinco minutos, observado o limite máximo de dez minutos diários. Se ultrapassado esse limite, será considerada como extra a totalidade do tempo que exceder a jornada normal, pois configurado tempo à disposição do empregador, não importando as atividades desenvolvidas pelo empregado ao longo do tempo residual (troca de uniforme, lanche, higiene pessoal, etc).

**Cancelada**. Resolução225/2025,art.1º,XV,2025_res0225.pdf:83–84:

> XV - Súmula nº366 (cancelada por perda de eficácia a partir de11/11/2017,pela Lei13.467/2017);

**Súmula437,I**,livro TRT3:7296–7300,página147:

> Após a edição da Lei nº8.923/94,a não concessão ou a concessão parcial do intervalo intrajornada mínimo, para repouso e alimentação, a empregados urbanos e rurais, implica o pagamento total do período correspondente, e não apenas daquele suprimido, com acréscimo de, no mínimo,50% sobre o valor da remuneração da hora normal de trabalho (art.71 da CLT),sem prejuízo do cômputo da efetiva jornada de labor para efeito de remuneração.

ItemIV,livro TRT3:7309–7312:

> Ultrapassada habitualmente a jornada de seis horas de trabalho, é devido o gozo do intervalo intrajornada mínimo de uma hora, obrigando o empregador a remunerar o período para descanso e alimentação não usufruído como extra, acrescido do respectivo adicional, na forma prevista no art.71,caput e§4º da CLT.

**Cancelada**. Resolução225/2025,art.1º,XXII,2025_res0225.pdf:99–100:

> XXII - Súmula nº437 (cancelada por perda de eficácia a partir de11/11/2017,pela Lei13.467/2017);

**OJSDI-197**,livro TRT3:8318–8320,página168:

> O adicional noturno integra a base de cálculo das horas extras prestadas no período noturno.

**OJSDI-1388**,livro TRT3:10520–10528,páginas206–207:

> O empregado submetido à jornada de12 horas de trabalho por36 de descanso, que compreenda a totalidade do período noturno, tem direito ao adicional noturno, relativo às horas trabalhadas após as5 horas da manhã.

**Vigência formal:** Súmulas60 e85 e OJs97 e388 constam sem cancelamento na edição oficial de2024 citada. Não estão na relação de cancelamentos da Resolução225/2025,artigos1º–6º,2025_res0225.pdf:51–136. Sua manutenção até31/10/2026 está **[NAO VERIFICADO]**, pois o livro atual do TST localizado em [livrointernet-12-pdf](https://www.tst.jus.br/documents/d/guest/livrointernet-12-pdf) bloqueou a leitura integral com403.

**Resultado delimitado da leitura:** nenhum dos seis verbetes acima afirma literalmente que as horas extras correspondem necessariamente aos últimos minutos trabalhados no dia, nem apresenta algoritmo para distribuir extras noturnas entre vários períodos intradiários; as referências são as linhas de cada verbete acima. A inexistência de qualquer outra Súmula ou OJ do TST com essa regra permanece **[NAO VERIFICADO]**.

## 31/10 15:49 · sessao 004f7cf3dbc6 · - · - · origem=Ronald (tmux)

Fonte primária oficial: [CLT atualizada — Câmara dos Deputados](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html). As linhas abaixo são da representação textual recuperada pela ferramenta web, sem arquivo local criado.

| Dispositivo | Arquivo:linha | Situação registrada |
|---|---|---|
| Art. 59, caput | `decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html:300` | Vigente; redação Lei 13.467/2017 |
| § 1º | mesmo arquivo:301 | Vigente; redação Lei 13.467/2017 |
| § 2º | mesmo arquivo:302 | Vigente; redação MP 2.164-41/2001 |
| § 3º | mesmo arquivo:303 | Vigente; redação Lei 13.467/2017 |
| § 4º | mesmo arquivo:304 | **Revogado pela Lei 13.467/2017** |
| § 5º | mesmo arquivo:306 | Vigente; incluído Lei 13.467/2017 |
| § 6º | mesmo arquivo:307 | Vigente; incluído Lei 13.467/2017 |
| Art. 73, caput | mesmo arquivo:360 | Texto vigente apresentado; redação DL 9.666/1946 |
| § 1º | mesmo arquivo:361 | Texto vigente apresentado; redação DL 9.666/1946 |
| § 2º | mesmo arquivo:363 | Texto vigente apresentado; redação DL 9.666/1946 |
| § 4º | mesmo arquivo:366 | Texto vigente apresentado; redação DL 9.666/1946 |
| § 5º | mesmo arquivo:368 | Texto vigente apresentado; redação DL 9.666/1946 |

Dois trechos literais centrais, totalizando 25 palavras da página:

- Art. 59, caput: “A duração diária do trabalho poderá ser acrescida de horas extras” — arquivo:300.
- Art. 73, § 5º: “Às prorrogações do trabalho noturno aplica-se o disposto neste capítulo.” — arquivo:368.

Conteúdo confirmado dos demais dispositivos, em paráfrase:

- Art. 59, § 1º: remuneração da extra pelo menos 50% superior à normal; § 2º: compensação do excesso de um dia pela diminuição em outro, com limites anual e diário; § 3º: extras não compensadas pagas na rescisão; §§ 5º e 6º: compensação semestral por acordo individual escrito e mensal por acordo individual tácito ou escrito. — arquivo:301–307.
- Art. 73, § 1º: hora noturna de 52 minutos e 30 segundos; § 2º: trabalho entre 22h de um dia e 5h do seguinte; § 4º: aplicação das disposições às horas noturnas nos horários mistos. — arquivo:361,363,366.
- Os dispositivos consultados **não contêm regra expressa identificando as extras como os últimos minutos cronológicos trabalhados**, nem regra específica para prorrogação situada em período anterior ao último. Essa constatação limita-se ao texto desses artigos. — arquivo:300–307,360–368.
- Apoio contextual: art. 58 estabelece duração normal diária; art. 71, § 2º exclui os intervalos de descanso da duração do trabalho. — arquivo:285,350.

Limitação operacional: tentativa de recuperação direta pelo shell falhou por resolução DNS; Planalto retornou bloqueio. A Câmara forneceu o texto oficial acessível. Nenhum arquivo foi criado, alterado ou apagado. [NAO VERIFICADO por arquivo:linha; evidenciado apenas pelas saídas das ferramentas]

## 31/10 15:48 · sessao 161e3a1c4479 · REPROVA-E-DESENHO-CODEX · verificador · origem=Code

**REPROVADO — PROCEDENTE, BLOQUEIA.** #8 e #9 continuam parcialmente abertos na atualização do estado antigo; também há caminhos que autorizam Codex sem leitura válida da triagem ou depois de a obra parar. (`bin/disjuntor.py:811`, `bin/disjuntor.py:816`, `bin/disjuntor.py:883`, `bin/disjuntor.py:989`; `bin/codex_pergunta.sh:54`, `bin/codex_pergunta.sh:166`)

“Morde” abaixo significa que o caso distingue estaticamente o comportamento de `8609b12c`. Execução integral do selo: **[não verificado]**; ele cria e remove arquivos. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:51`)

| Achado v2 | Está curado? | O selo morde a volta ao comportamento antigo? |
|---|---|---|
| **#1** | **Sim, no cenário relatado. Não PROCEDENTE, não-bloqueia.** A porta executa a decisão, incluindo triagens pendentes. (`bin/disjuntor.py:982`) | **Sim.** A quarta chamada passa pelo wrapper sem Stop/PreToolUse intermediário; exige rc 3 e ausência de resposta. Mutante restaura consulta apenas ao estado. (`bin/tests/test_disjuntor.sh:1216`, `bin/tests/test_disjuntor.sh:1224`, `bin/tests/test_disjuntor.sh:1457`) |
| **#2** | **Sim. Não PROCEDENTE, não-bloqueia.** A mesma decisão lê e aplica a liberação válida. (`bin/disjuntor.py:906`, `bin/disjuntor.py:982`) | **Sim.** Escreve DESENHO após disparar e chama diretamente o wrapper; a base continuaria negando. (`bin/tests/test_disjuntor.sh:1229`, `bin/tests/test_disjuntor.sh:1235`, `bin/tests/test_disjuntor.sh:1458`) |
| **#3** | **Sim, para os casos relatados. Não PROCEDENTE, não-bloqueia.** `shlex` preserva valores completos e a última atribuição prevalece. (`bin/disjuntor.py:1254`, `bin/disjuntor.py:1261`) | **Sim.** Exercita atribuição repetida, texto dentro de `DUMMY` e `O9 (C)`. (`bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1262`, `bin/tests/test_disjuntor.sh:1263`, `bin/tests/test_disjuntor.sh:1464`) |
| **#6** | **Sim, com fonte íntegra. Não PROCEDENTE nesse cenário, não-bloqueia.** Liberação e rearme arquivam todas as identidades triadas, incluindo descartadas. (`bin/disjuntor.py:913`, `bin/disjuntor.py:958`) | **Sim.** Reclassifica antes do reset e depois acrescenta duas novas voltas; a base recontaria a antiga. (`bin/tests/test_disjuntor.sh:1273`, `bin/tests/test_disjuntor.sh:1277`, `bin/tests/test_disjuntor.sh:1281`, `bin/tests/test_disjuntor.sh:1466`) |
| **#7** | **Sim. Não PROCEDENTE, não-bloqueia.** O rearme alcança obra que só possui triagens. (`bin/disjuntor.py:951`, `bin/disjuntor.py:952`) | **Sim.** Rearma antes de qualquer decisão B3; a base recusaria. (`bin/tests/test_disjuntor.sh:1287`, `bin/tests/test_disjuntor.sh:1291`, `bin/tests/test_disjuntor.sh:1469`) |
| **#8** | **Parcial. PROCEDENTE, BLOQUEIA.** Ausência só é reconhecida como perda quando o contador novo já é positivo. (`bin/disjuntor.py:811`, `bin/disjuntor.py:816`) | **Sim no percurso novo; falta o estado herdado.** O caso cria o contador pela versão nova antes de retirar a fonte. (`bin/tests/test_disjuntor.sh:1299`, `bin/tests/test_disjuntor.sh:1301`, `bin/tests/test_disjuntor.sh:1470`) |
| **#9** | **Parcial. PROCEDENTE, BLOQUEIA.** Rejeita corrupção estrutural e truncamento, mas a perda de linhas completas antes do primeiro contador novo passa. (`bin/disjuntor.py:819`, `bin/disjuntor.py:822`, `bin/disjuntor.py:828`) | **Sim para as variantes cobertas; falta a redução na migração.** O teste semeia o contador antes de remover linhas. (`bin/tests/test_disjuntor.sh:1314`, `bin/tests/test_disjuntor.sh:1324`, `bin/tests/test_disjuntor.sh:1326`, `bin/tests/test_disjuntor.sh:1472`) |
| **#10** | **Sim. Não PROCEDENTE, não-bloqueia.** Leitura, idempotência e append participam da trava do estado. (`bin/disjuntor.py:786`, `bin/disjuntor.py:800`) | **Sim.** Mantém a trava ocupada e exige recusa sem append; depois exige exatamente uma linha. (`bin/tests/test_disjuntor.sh:1344`, `bin/tests/test_disjuntor.sh:1346`, `bin/tests/test_disjuntor.sh:1350`, `bin/tests/test_disjuntor.sh:1475`) |
| **#12** | **Sim, nos cenários relatados. Não PROCEDENTE, não-bloqueia.** A guarda antecede a remoção; sidecar restante impede a chamada. (`bin/codex_pergunta.sh:54`, `bin/codex_pergunta.sh:165`) | **Sim.** Confere preservação após negativa e recusa quando o sidecar não pode ser removido. (`bin/tests/test_disjuntor.sh:1245`, `bin/tests/test_disjuntor.sh:1246`, `bin/tests/test_disjuntor.sh:1249`, `bin/tests/test_disjuntor.sh:1459`, `bin/tests/test_disjuntor.sh:1463`) |

O resíduo de **#8** é o próprio cenário antigo na atualização: JSON com `codex=[a,b,c]`, obra disparada e sem `b3_triagem_linhas`; a fonte ausente vira `{}`, apaga `codex`, permite liberar arquivando uma lista vazia e deixa as mesmas três voltas dispararem quando a fonte retorna. (`bin/disjuntor.py:811`, `bin/disjuntor.py:816`, `bin/disjuntor.py:883`, `bin/disjuntor.py:913`, `bin/disjuntor.py:888`)

O resíduo de **#9**: nesse estado herdado sem contador, uma fonte reduzida de três para duas linhas completas é aceita e substitui a contagem conhecida sem FALHA. (`bin/disjuntor.py:811`, `bin/disjuntor.py:822`, `bin/disjuntor.py:830`, `bin/disjuntor.py:883`)

Há ainda uma limitação dos negativos do selo: `_xp` procura o recado de bloqueio; sua ausência também pode resultar de falha aberta. Isso afeta os negativos de reset/rearme e parte dos casos de corrupção. (`bin/tests/test_disjuntor.sh:1203`, `bin/tests/test_disjuntor.sh:1277`, `bin/tests/test_disjuntor.sh:1284`, `bin/tests/test_disjuntor.sh:1292`, `bin/tests/test_disjuntor.sh:1315`, `bin/tests/test_disjuntor.sh:1321`)

**(a)** O escritor físico do JSON é `_com_estado`: toma `flock`, chama o mutador e substitui o arquivo ainda sob a trava. As leituras gerais ocorrem nele e em `ler_estado`. (`bin/disjuntor.py:249`, `bin/disjuntor.py:253`, `bin/disjuntor.py:268`, `bin/disjuntor.py:274`, `bin/disjuntor.py:288`, `bin/disjuntor.py:294`)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `b1[agente].checado/disparado/motivo` | `b1_do_agente`. (`bin/disjuntor.py:481`) | Cache e decisão; `b1_disparados` lê `disparado`. (`bin/disjuntor.py:476`, `bin/disjuntor.py:478`, `bin/disjuntor.py:479`, `bin/disjuntor.py:490`) |
| `b2.ultimo` | `b2_decide`. (`bin/disjuntor.py:1134`) | `b2_decide`. (`bin/disjuntor.py:1133`) |
| `b4_para.desde` | `b4_stop`, incluindo remoção. (`bin/disjuntor.py:1149`, `bin/disjuntor.py:1155`) | `b4_stop`. (`bin/disjuntor.py:1157`) |
| `b4.negou_desde` | `pretool`, incluindo remoção. (`bin/disjuntor.py:1374`, `bin/disjuntor.py:1379`) | `pretool`. (`bin/disjuntor.py:1373`, `bin/disjuntor.py:1378`) |
| `b3_limiar` | `_migra_limiar`. (`bin/disjuntor.py:710`) | `_migra_limiar`. (`bin/disjuntor.py:699`, `bin/disjuntor.py:707`) |
| `b3_sem_obra` | `decide_b3`. (`bin/disjuntor.py:871`) | Deduplicação e `_b3_sem_obra`. (`bin/disjuntor.py:870`, `bin/disjuntor.py:1518`) |
| `b3[obra].reprovados` | Migração, decisão e rearme. (`bin/disjuntor.py:709`, `bin/disjuntor.py:877`, `bin/disjuntor.py:914`, `bin/disjuntor.py:959`) | Os mesmos percursos. (`bin/disjuntor.py:705`, `bin/disjuntor.py:886`, `bin/disjuntor.py:912`, `bin/disjuntor.py:957`) |
| `b3[obra].disparado` | Decisão e rearme. (`bin/disjuntor.py:889`, `bin/disjuntor.py:914`, `bin/disjuntor.py:959`) | Migração e decisão. (`bin/disjuntor.py:702`, `bin/disjuntor.py:888`, `bin/disjuntor.py:898`) |
| `b3[obra].codex` | Decisão e rearme. (`bin/disjuntor.py:883`, `bin/disjuntor.py:914`, `bin/disjuntor.py:959`) | Decisão e rearme. (`bin/disjuntor.py:887`, `bin/disjuntor.py:913`, `bin/disjuntor.py:955`) |
| `b3[obra].rearmado` | Migração e rearme. (`bin/disjuntor.py:708`, `bin/disjuntor.py:957`) | Decisão. (`bin/disjuntor.py:874`, `bin/disjuntor.py:881`) |
| `b3[obra].liberado` | Decisão. (`bin/disjuntor.py:912`) | Decisão. (`bin/disjuntor.py:874`, `bin/disjuntor.py:881`, `bin/disjuntor.py:912`) |
| `b3[obra].consumidas` | Decisão. (`bin/disjuntor.py:915`) | Decisão. (`bin/disjuntor.py:908`) |
| `b3[obra].disparado_em` | `_fotografa`. (`bin/disjuntor.py:728`, `bin/disjuntor.py:730`) | Migração, fotografia e liberação. (`bin/disjuntor.py:703`, `bin/disjuntor.py:728`, `bin/disjuntor.py:910`) |
| `b3[obra].topo_no_disparo` | `_fotografa`. (`bin/disjuntor.py:727`, `bin/disjuntor.py:730`) | Decisão. (`bin/disjuntor.py:903`, `bin/disjuntor.py:908`) |
| `b3_triagem_linhas` | **`_triagens` e `triagem`.** (`bin/disjuntor.py:830`, `bin/disjuntor.py:797`) | Ambos. (`bin/disjuntor.py:811`, `bin/disjuntor.py:797`) |

Portanto, **há múltiplos escritores lógicos**, inclusive do contador novo; nesses percursos, todos passam pela mesma trava. Escrita fora da trava do JSON: **não PROCEDENTE, não-bloqueia**. (`bin/disjuntor.py:486`, `bin/disjuntor.py:800`, `bin/disjuntor.py:935`, `bin/disjuntor.py:971`, `bin/disjuntor.py:1136`, `bin/disjuntor.py:1162`, `bin/disjuntor.py:1382`)

Na trilha `triagem.log`, os quatro campos têm um escritor: `triagem`, sob essa trava. O leitor é `_triagens`; timestamp é separado, mas não validado nem usado; obra e volta são as chaves, e o último veredito prevalece. Idempotência, decisão e rearme consomem esse mapa. **Escritor fora da trava: não PROCEDENTE, não-bloqueia.** (`bin/disjuntor.py:794`, `bin/disjuntor.py:795`, `bin/disjuntor.py:800`, `bin/disjuntor.py:826`, `bin/disjuntor.py:829`, `bin/disjuntor.py:790`, `bin/disjuntor.py:883`, `bin/disjuntor.py:958`)

O sidecar fornece `obra/papel/hash/pid/fim/sha`: o wrapper escreve; `_le_id` lê; `triagem` verifica papel/SHA e compõe a identidade. A validação continua estrutural, preservando #14 sobre autoria. (`bin/codex_pergunta.sh:188`; `bin/disjuntor.py:757`, `bin/disjuntor.py:760`, `bin/disjuntor.py:780`, `bin/disjuntor.py:782`, `bin/disjuntor.py:784`)

**(b)** A liberação normal exige linha reconhecida, não consumida, fora da fotografia e posterior ao disparo; falha de persistência invalida a liberação. O rearme administrativo explicitamente zera a parada sem DESENHO/ERRO-BOBO. (`bin/disjuntor.py:906`, `bin/disjuntor.py:908`, `bin/disjuntor.py:910`, `bin/disjuntor.py:936`, `bin/disjuntor.py:959`)

Entretanto, **há passagem indevida — PROCEDENTE, BLOQUEIA**:

- **Codex depois de a obra parar:** a guarda ocorre antes da espera por slot. Outra decisão pode parar a obra durante essa espera; a chamada não repete a consulta. (`bin/codex_pergunta.sh:54`, `bin/codex_pergunta.sh:146`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:166`)
- **Opus final com obra parada:** exceção no PreToolUse libera a ferramenta; classificação textual ou obra não reconhecida pode evitar a consulta B3. Persistem #4 e #5. (`bin/disjuntor.py:501`, `bin/disjuntor.py:513`, `bin/disjuntor.py:1367`, `bin/disjuntor.py:1370`, `bin/disjuntor.py:1406`)
- **Identidade incorreta alimentando a decisão:** chamadas concorrentes compartilham resposta/sidecar sem trava por caminho; `mtime` continua sendo o critério de resposta nova. Persistem #11 e #13. (`bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:166`, `bin/codex_pergunta.sh:187`, `bin/codex_pergunta.sh:188`)

**(c) Sim — PROCEDENTE, BLOQUEIA.**

- **Autorização sem ler a triagem:** `_triagens=None` registra FALHA e preserva o cache, mas não torna a porta “não sei”. Sem disparo anterior, com cache vazio ou inferior a três, a decisão retorna livre; `b3_parada` devolve zero e o wrapper prossegue. (`bin/disjuntor.py:861`, `bin/disjuntor.py:864`, `bin/disjuntor.py:882`, `bin/disjuntor.py:898`, `bin/disjuntor.py:989`; `bin/codex_pergunta.sh:54`)
- **Fonte ausente tomada como vazia:** sem contador novo, ausência vira `{}`, mesmo com contagem herdada conhecida; estado ausente também vira `{}`. (`bin/disjuntor.py:263`, `bin/disjuntor.py:297`, `bin/disjuntor.py:811`, `bin/disjuntor.py:816`)
- **Painel com zeros sem leitura:** disparos, negações e liberações começam zerados; falha de abertura e linhas não parseadas são ignoradas; a apresentação imprime os zeros. Persiste #15. (`bin/disjuntor.py:1450`, `bin/disjuntor.py:1452`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1464`, `bin/disjuntor.py:1482`, `bin/disjuntor.py:1582`)
- **Rc mascarado pelo hook:** a exceção é registrada, mas `main_pretool` retorna zero sem emitir negativa. No wrapper, especificamente, rc não zero de `--b3-parada` é recusado; o falso verde acima chega como rc zero. (`bin/disjuntor.py:1406`, `bin/disjuntor.py:1408`, `bin/disjuntor.py:989`; `bin/codex_pergunta.sh:54`)

_1123 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
