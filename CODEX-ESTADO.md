# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 15:17** · publicado em **27/11/2026 15:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 15:17 · sessao d62db9fabea1 · INVENTARIO-PELA-REGRA · leitura · origem=Code

# nascentes.md

Inventário do lote 2 entregue no chat, em leitura exclusiva. A identidade da cópia com o hash `59600163ccc9cffb8f3ae148beede7cd7866ae86` ficou [nao verificado]. As referências abaixo usam as linhas da cópia, que diferem das do anexo. Por exemplo, `ESMERIL-SEM-DIA-DO-TURNO` está em `app/docs/BACKLOG.md:2121`.

**Ainda vivo?** refere-se ao caminho de código citado. Ocorrências, estoques, volumes e execução em produção ficam [nao verificado] quando a sondagem fornecida não os demonstra. **Morre?** indica quanto da entrega desapareceria com a correção da origem, inclusive quando a cópia já mostra essa correção. O critério de origem é a autoridade de leitura ou a porta de escrita. `CLAUDE.md:20`, `CLAUDE.md:32`.

### Referências do placar

Na coluna `placar20`, **juiz** corresponde a `um juiz por pergunta` e **escritor** a `um escritor por entidade`. A indicação identifica o contrato aplicável à origem. As notas indicam quando o detalhe local fica fora do alcance do selo. `app/core/contratos_estruturais.py:55`, `app/core/contratos_estruturais.py:1038`.

| Família | Juiz | Escritor |
|---|---|---|
| batida | `app/core/contratos_estruturais.py:375` | `app/core/contratos_estruturais.py:91` |
| turno/marcos | `app/core/contratos_estruturais.py:225` | `app/core/contratos_estruturais.py:102` |
| célula/precedência | `app/core/contratos_estruturais.py:165` | `app/core/contratos_estruturais.py:113` |
| ausência/férias | `app/core/contratos_estruturais.py:247` | `app/core/contratos_estruturais.py:126` |
| escala | `app/core/contratos_estruturais.py:444` | `app/core/contratos_estruturais.py:139` |
| chamado | `app/core/contratos_estruturais.py:262` | `app/core/contratos_estruturais.py:496` |
| folha/export | `app/core/contratos_estruturais.py:353` | `app/core/contratos_estruturais.py:151` |

## Inventário

| id | nascente | sítio | morre? | ainda vivo? | placar20 | nota |
|---|---|---|---|---|---|---|
| ESMERIL-SEM-DIA-DO-TURNO | Dia do turno derivado fora do juiz | `app/ponto/turnos.py:3014`, `app/ponto/services/regularizacao.py:61`, `app/ponto/services/regularizacao.py:73` | sim | sim | turno/marcos · juiz | A regularização obtém o turno da autoridade e usa o dia civil da entrada na escrita. |
| TIPO-V5-NAO-BLOQUEANTES | Dia do turno derivado fora do juiz + Relógio do vigia de células relido no interior | `app/ponto/services/vigia_pareador_celula.py:109`, `app/ponto/management/commands/tripwire_celulas.py:85`, `app/ponto/management/commands/tripwire_celulas.py:247` | parcial por nascente | sim | turno/marcos · juiz para o dia, relógio fora do placar | A comparação civil e as leituras internas de tempo têm sítios distintos. |
| VIGIA-PAREADOR-DIA-CIVIL | Dia do turno derivado fora do juiz | `app/ponto/services/vigia_pareador_celula.py:97`, `app/ponto/services/vigia_pareador_celula.py:109` | sim | sim | turno/marcos · juiz | O vigia seleciona o turno por `data_turno` e procura a órfã na chave civil. |
| SAIDA-NOTURNA-NO-DIA-CERTO | Instante do marco noturno fora da cadeia + Colisão do plantio sem prova da lâmpada | `app/chamados/juizes.py:301`, `app/chamados/services/disputa_emissao.py:1222`, `app/chamados/services/disputa_emissao.py:1260`, `app/chamados/services/materializacao.py:1790` | parcial por nascente | sim nos sítios, caso [nao verificado] | turno/marcos · juiz, chamado · escritor | O texto usa o dia da cobrança. O outro ramo carimba colisão sem a prova usada no ramo de paridade. |
| [nome]-NOTURNO-DIA-CIVIL | Dia da pergunta gravado fora da porta do dia do turno | `app/chamados/models.py:3179`, `app/chamados/models.py:3192`, `app/chamados/services/fio.py:1963`, `app/docs/BACKLOG.md:1196` | parcial | não no nascimento com batida de origem, acervo [nao verificado] | chamado · escritor, turno/marcos · juiz | Identificação e retratação do estoque são atos próprios. |
| FICHA-LE-O-CARIMBO | Vínculo do dia sem a célula geradora | `app/colaboradores/services/ficha.py:126`, `app/colaboradores/services/ficha.py:129`, `app/escala/alimentacao.py:228` | parcial | sim | escala · juiz | A ficha consulta a autoridade sem alimentar o carimbo nesse trecho. O teto de custo continua sendo uma entrega. |
| ABSOLVIDA-PELA-FOLGA | Dia da pergunta gravado fora da porta do dia do turno | `app/chamados/services/fio.py:1139`, `app/chamados/services/fio.py:1963`, `app/chamados/reconciliador.py:373`, `app/docs/BACKLOG.md:2423` | parcial | não no nascimento com batida de origem, acervo [nao verificado] | chamado · escritor, turno/marcos · juiz | Absolvições antigas e exportadas ainda exigem decisão sobre o estoque. |
| BUG-PLANTIO-NOTURNO-MAIS-1 | Dia da pergunta gravado fora da porta do dia do turno | `app/chamados/models.py:3192`, `app/chamados/services/disputa_emissao.py:393`, `app/chamados/juizes.py:311`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:416` | parcial | não no ramo com origem, migração [nao verificado] | chamado · escritor, turno/marcos · juiz | A data do episódio tem um significado no nascimento. A migração do legado é separada. |
| CONTRATO-DE-ENTRADA-DO-FIO | Batida de origem fora do envelope da pergunta | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`, `app/chamados/services/disputa_emissao.py:300`, `app/chamados/models.py:3183`, `app/chamados/models.py:3914` | parcial | não no emissor conferido, alcance dos demais [nao verificado] | chamado · escritor | O emissor conferido entrega a batida do ato. |
| PLANTIO-SILENCIO-SEM-DIA | independente | `app/chamados/services/disputa_emissao.py:433`, `app/chamados/services/disputa_emissao.py:439` | não | sim | fora do placar | A saída sem dia já é decidida. A entrega é dar destino visível à recusa hoje registrada em log. |
| SELFIE-RETRO-SEM-ULTIMA-BATIDA | Batida de origem fora do envelope da pergunta | `app/ponto/management/commands/marcar_foto_ausente_retro.py:98`, `app/api/foto_recebida.py:38`, `app/api/foto_recebida.py:99` | parcial | não para o nascimento pelo emissor conferido, legado [nao verificado] | chamado · escritor | A recepção usa a origem do episódio. O caso legado sem origem tem tratamento próprio. |
| E4B-D2-ANCORA-RAIOX | Âncora do fio recalculada no Raio-X | `app/chamados/services/raiox.py:149`, `app/chamados/services/raiox.py:157`, `app/chamados/services/fio.py:97`, `app/chamados/services/fio.py:117` | sim | sim | chamado · juiz | O diagnóstico troca o destino pelo pai conforme `VIVOS`. O fio tem sua própria resolução da âncora. |
| E4B-D3-ARQUIVAR-PRESCRITOS | independente | `app/config/crons.py:1477`, `app/chamados/management/commands/arquivar_prescritos.py:47` | não | não sei [nao verificado] | fora do placar | A obra mede pendências sem dono. A configuração marca o cron como desligado. |
| E4B-FILA-OPUS-C14 | Dia da pergunta lido cru no backfill | `app/chamados/management/commands/backfill_fio_morto_e4b.py:188`, `app/chamados/juizes.py:339`, `app/chamados/services/disputa_emissao.py:1955` | parcial | sim | chamado · juiz, C6/C20 | O reparador ainda abre a data diretamente. A fila também trata identidades e provas. |
| C6-RELOGIO-BORDA | Relógio da pergunta relido no interior | `app/chamados/juizes.py:934`, `app/chamados/juizes.py:1569`, `app/chamados/reconciliador.py:1210`, `app/chamados/reconciliador.py:1214` | sim | sim | fora do placar | O reconciliador já tem `agora`. Uma chamada a C6 ainda usa o relógio implícito. C6 é contrato de chamado, a propagação do instante é o resíduo. |
| CAL-DOIS-RELOGIOS | Relógio do calendário lido duas vezes | `app/colaboradores/services/calendario.py:192`, `app/colaboradores/services/calendario.py:279` | sim | não | fora do placar | O calendário e o retrato usam o mesmo `_agora`. |
| DETECTOR-SO-A-1A-PAUSA | Marcos de pausa truncados fora da sequência completa | `app/chamados/detector_anomalias.py:199`, `app/escala/servico_jornada.py:76`, `app/escala/models.py:1177` | parcial | sim | turno/marcos · juiz | A cadeia lida pelo detector leva uma pausa. A autoridade da célula expõe todas. O item do papel da batida é adicional. |
| F5-RELOGIO-DO-EXPORT | Relógio da retenção relido na classificação | `app/folha/fora_da_folha.py:137`, `app/folha/export.py:977`, `app/folha/export.py:1247` | sim | sim | fora do placar | O juiz da retenção integra folha/export. O resíduo é a classificação perder o `hoje` que a outra leitura já recebeu. |
| LASTRO-PARTE-2 | Fecho de cobrança pelo marco fora do evento da célula | `app/ponto/services/lastro.py:364`, `app/ponto/services/lastro.py:394`, `app/chamados/reconciliador.py:1533`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:49` | sim | não | chamado · juiz e escritor | O evento já reconcilia. O comando opera como vigia. |
| LASTRO-PARTE-2-FILA-OPUS | Lâmpada acesa tomada por batida | `app/ponto/services/lastro.py:327`, `app/ponto/services/lastro.py:340`, `app/ponto/services/diagnostico_escala.py:155`, `app/ponto/status_dia.py:186` | parcial | sim | célula/precedência · juiz | Ainda há leitores que usam o sinal ou a hora da lâmpada como prova. Gatilho, trilha e testes têm escopo adicional. |
| O127 | Marcos de pausa truncados fora da sequência completa | `app/ponto/services/flip_auto.py:201`, `app/ponto/services/flip_auto.py:218`, `app/escala/models.py:1177` | sim | sim | turno/marcos · juiz | O flip lê as pausas e ainda usa só a primeira ao montar o slot. |
| PAR-NAO-ADJACENTE | Papel do par descartado pela exigência de adjacência | `app/escala/utils.py:444`, `app/escala/utils.py:467`, `app/ponto/turnos.py:502`, `app/ponto/turnos.py:549` | sim | sim | turno/marcos · juiz | O alinhador exige que as pontas sejam vizinhas mesmo após o pareador reconhecê-las. |
| R1 | Papel do par fora da ata | `app/escala/utils.py:606`, `app/escala/utils.py:646`, `app/ponto/turnos.py:1565`, `app/docs/BACKLOG.md:1310` | parcial | não sei para o caso restante [nao verificado] | turno/marcos · juiz | O alinhamento conserva ramo por distância. A incidência dele no caso residual ficou [nao verificado]. |
| RELOGIO-NA-BORDA | Relógio do cadastro relido no interior | `app/colaboradores/services/vinculo.py:194`, `app/colaboradores/services/pos_vinculo.py:56`, `app/escala/signals.py:67`, `app/escala/signals.py:119` | sim | sim | fora do placar | A borda e os serviços internos consultam o tempo no mesmo fluxo de cadastro. |
| RELOGIO-NA-BORDA-OPUS-C5E | Relógio do cadastro relido no interior | `app/escala/management/commands/corrigir_vinculo_vigencia.py:48`, `app/escala/management/commands/corrigir_vinculo_vigencia.py:86`, `app/colaboradores/services/vinculo.py:742`, `app/ponto/portas/celula.py:482` | parcial | sim para relógio | fora do placar | A porta da célula já recebe `hoje`. A fila também inclui transação e prova da catraca. |
| RETRATO-DOIS-RELOGIOS | Relógio da retenção relido na classificação | `app/folha/fora_da_folha.py:125`, `app/folha/fora_da_folha.py:137`, `app/folha/export.py:977`, `app/folha/export.py:1247` | sim | sim | fora do placar | O retrato lê os dias com o `hoje` da borda. A classificação da mesma competência pode usar outra data. |
| TETO-NA-LISTA-DOIS-RELOGIOS | Relógio do teto da lista relido na medição | `app/ponto/management/commands/lavrar_teto_do_dia.py:27`, `app/ponto/services/teto_na_lista.py:49`, `app/ponto/services/teto_na_lista.py:72` | sim | sim | fora do placar | O comando passa a data e a medição obtém o próprio instante para os insumos. |
| PASSIVO-VIVA-EM-DISPUTA-FECHADA | independente | `app/chamados/models.py:2047`, `app/chamados/models.py:2122`, `app/chamados/services/validacao.py:830`, `app/docs/BACKLOG.md:1210` | não | não sei [nao verificado] | fora do placar | É tratamento de acervo. A possibilidade de fechar e a vitalidade têm autoridades nas portas atuais. |
| ATA-X-TURNO-PAUSA | Papel do par fora da ata | `app/ponto/services/cartorio.py:1205`, `app/escala/utils.py:467`, `app/escala/utils.py:588`, `app/escala/utils.py:1748`, `app/ponto/turnos.py:1850` | parcial | sim para realinhamento residual, caso [nao verificado] | turno/marcos · juiz | O montador já lê o pareador. A guarda do marco mais próximo ainda pode retirar a batida de um marco. |
| POSICAO-TOTAL-ATE-A-DATA | Total do espelho até a data sem juiz | `app/ponto/services/espelho.py:1230`, `app/relatorios/cartao_pela_celula.py:296`, `app/ponto/services/dia_pago.py:752`, `app/docs/BACKLOG.md:1181` | sim | sim | fora do placar | Falta o total parcial com a semântica do resumo do espelho. |
| O103 | independente | `app/ponto/portas/celula.py:112`, `app/ponto/portas/celula.py:765`, `app/docs/BACKLOG.md:1382` | não | não no recorte de código, TXT [nao verificado] | fora do placar | O modo dia usa a mesma porta com horizonte limitado. A retificação do TXT é outro ato. |
| FECHAMENTO-ZERO-COM-BATIDA | independente | `app/folha/export.py:968`, `app/ponto/services/espelho.py:1007`, `app/docs/BACKLOG.md:1419` | não | não sei [nao verificado] | fora do placar | É encaminhamento de cadastro sem jornada descrita ou de batida sem par. |
| O238 | Testemunha do fechamento por carimbo e hash duplicado | `app/ponto/management/commands/carimbo_gravado.py:29`, `app/ponto/portas/regen_exportada.py:37`, `app/ponto/portas/regen_exportada.py:204`, `app/ponto/services/fechamento.py:1637` | parcial | sim | fora do placar | O instrumento define a própria foto para hash. O identificador da trilha é outro item. |
| O145 | Ponta de HE julgada pela grade | `app/ponto/janela_he.py:203`, `app/ponto/services/espelho.py:741`, `app/ponto/services/espelho.py:754`, `app/docs/BACKLOG.md:1302` | parcial | não na origem, resíduos [nao verificado] | fora do placar | O espelho já recebe a ponta órfã do motor. A pergunta específica de HE está fora do selo de geometria. |
| QUARENTENA-DO-COLAB | Quarentena de cobrança por dia sem autoridade e porta | `app/chamados/services/painel.py:158`, `app/chamados/services/painel.py:188`, `app/chamados/models.py:527`, `app/docs/BACKLOG.md:900` | parcial | sim como lacuna | fora do placar | Há silêncio por chamado. O estado por pessoa e período pedido pela obra carece de representação e porta próprias. |
| BO-354 | Papel do par fora da ata | `app/chamados/detector_anomalias.py:31`, `app/ponto/utils_turno_orfao.py:20`, `app/ponto/utils_turno_orfao.py:31`, `app/ponto/turnos.py:1850` | parcial | sim no leitor, três casos [nao verificado] | turno/marcos · juiz | O emissor delega, mas o helper usa pareamento genérico sem marcos do vínculo. |
| CORTE-B-30 | independente | `app/ponto/management/commands/aplicar_09_corte_b.py:228`, `app/ponto/management/commands/aplicar_09_corte_b.py:239`, `app/docs/BACKLOG.md:1299` | não | não sei [nao verificado] | fora do placar | É lista de explicações do lote. O comando já declara as guardas de separação. |
| DESLIGAMENTO-NAO-CARIMBA-RESPONDIDA | Destino da respondida no desligamento sem ato próprio | `app/chamados/models.py:2044`, `app/chamados/models.py:2123`, `app/colaboradores/services/desligamento.py:165`, `app/docs/BACKLOG.md:1211` | não, solução substituída | não para o carimbo apontado | chamado · escritor | O caminho atual preserva a resposta. A demanda vigente está na sucessora. |
| DESLIGAR-LIQUIDA-TUDO | Destino da respondida no desligamento sem ato próprio | `app/colaboradores/services/desligamento.py:164`, `app/colaboradores/services/desligamento.py:187`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`, `app/chamados/models.py:2123` | parcial | sim | chamado · escritor | Manual e cron convergem na porta. A guarda preserva a resposta, sem o ato específico de encaminhamento previsto na obra. |
| O210-COL417-COL931 | independente | `app/ponto/management/commands/recalcular_fechamento.py:110`, `app/ponto/services/fechamento.py:468`, `app/docs/BACKLOG.md:1213` | não | não sei [nao verificado] | fora do placar | É reapuração de dois gravados antigos pela porta existente. |
| O232 | Papel do par fora da ata | `app/escala/utils.py:1999`, `app/escala/utils.py:2053`, `app/escala/tests/test_saida_valida_cross_meia_noite.py:60`, `app/config/settings/base.py:165` | parcial | não no ramo padrão para o caso descrito | turno/marcos · juiz | O ramo usa a saída do turno fechado e a remove do dia seguinte. Impacto na frota [nao verificado]. |
| O73b | Volta de pausa julgada como nova entrada | `app/ponto/motor_calculo_v2.py:2777`, `app/ponto/motor_calculo_v2.py:2805`, `app/ponto/motor_calculo_v2.py:2822`, `app/ponto/tests/test_o73_volta_do_intervalo.py:60` | parcial | não na origem, gravado de 09 [nao verificado] | turno/marcos · juiz | O motor agrupa pelo dia da jornada e zera o atraso dos blocos internos. |
| POSICAO-PORTA-POR-MOTIVO | Porta da retenção por motivo sem projeção comum | `app/folha/fora_da_folha.py:278`, `app/folha/fora_da_folha.py:302`, `app/folha/retrato_folha.py:1024`, `app/chamados/services/tarefa_do_dia.py:809` | parcial | sim para motivo geral | chamado · juiz C19 para a ação do dia, motivo geral fora do placar | A linha diária recebe porta. O motivo geral é publicado sem a mesma projeção de ação. |
| SITUACAO-LEITOR-CRU | Desligamento derivado fora do juiz | `app/colaboradores/services/desligamento.py:23`, `app/ponto/services/cartorio.py:1803`, `app/ponto/services/cartorio.py:1868`, `app/ponto/services/vigia_de_hora.py:166`, `app/core/juizes.py:283` | sim | sim | ausência/férias · juiz | Consumidores ainda leem `situacao` diretamente. |
| COL882-O-PROXIMO-COL515 | independente | `app/ponto/supra_juiz.py:155`, `app/ponto/services/vinculo_na_lista.py:32`, `app/docs/BACKLOG.md:1274` | não | não sei [nao verificado] | fora do placar | É correção de cadastro para dias sem cobertura de vínculo. |
| O143 | independente | `app/ponto/supra_juiz.py:467`, `app/chamados/services/fabrica_da_celula.py:317`, `app/chamados/services/fabrica_da_celula.py:365`, `app/docs/BACKLOG.md:1535` | não | não sei para o encaminhamento com prazo [nao verificado] | fora do placar | A classe já tem julgamento e escritor de pauta. A entrega é fluxo administrativo. |
| BO-0611-05-DESLIGADO-FERIAS | independente | `app/api/views.py:394`, `app/api/views.py:405`, `app/docs/BACKLOG.md:2324` | não | não sei para o efeito financeiro pedido [nao verificado] | fora do placar | O comportamento observado é recusa do registro. A obra pede decisão sobre o tratamento financeiro. |
| E4-8-ROTULO-OK | independente | `app/ponto/status_dia.py:31`, `app/ponto/status_dia.py:95`, `app/ponto/portas/celula.py:1061`, `app/docs/BACKLOG.md:2518` | não | sim | fora do placar | A palavra `ok` ainda vem com o texto de que todas as batidas foram registradas. |
| EMIS-FALSE-ATA-TRABALHO | Lavra sem invariante entre `trabalha` e `tipo_dia` | `app/ponto/services/cartorio.py:757`, `app/ponto/services/cartorio.py:1470`, `app/escala/utils.py:1499`, `app/escala/utils.py:1857`, `app/chamados/juizes.py:503` | parcial | sim para a combinação no caminho intermitente, estoque [nao verificado] | célula/precedência · escritor, chamado · juiz C18 | A grade intermitente pode usar batida para classificar trabalho. A cobrança aplica seu juiz sobre ambos os insumos. |
| EMIS-PAUTA-DEDUP | Pauta do limbo sem unicidade atômica | `app/chamados/services/fabrica_da_celula.py:251`, `app/chamados/services/fabrica_da_celula.py:277`, `app/chamados/services/fabrica_da_celula.py:365`, `app/pautas/models.py:123` | sim | sim | fora do placar | Uma consulta anterior à escrita pode ser repetida por outro julgamento. A trava de outra pauta dá o contraste no próprio código. |
| F3-PORTA-E-CRON-PRAZO | independente | `app/config/crons.py:822`, `app/config/crons.py:837`, `app/config/crons.py:2040`, `app/ponto/services/espelho.py:1065`, `app/docs/BACKLOG.md:1931` | não | não sei quanto ao estoque [nao verificado] | fora do placar | É reencaminhamento e organização de tarefas. A cópia tem os crons e a injeção de feriados. |
| ISENTO-H10-MENORES | Isenção corrente lida fora do predicado | `app/core/constants.py:13`, `app/ponto/services/cartorio.py:954`, `app/ponto/services/cartorio.py:1303`, `app/chamados/veredito_lavrado.py:231`, `app/docs/BACKLOG.md:1077` | parcial | sim | fora

[truncado em 20000 de 88507 caracteres]

## 27/11 15:16 · sessao 4b6ad359cc31 · INVENTARIO-PELA-REGRA · leitura · origem=Code

# nascentes.md — INVENTARIO-PELA-REGRA v2, parte 3, lote 1

Inventário dos **179 IDs fornecidos**. São 158 linhas do bloco principal e 21 complementos que também vieram no anexo. Quando o ponteiro da GRADE estava deslocado, localizei a entrada pelo ID. Usei a cópia disponível em modo somente leitura. A correspondência da cópia com o hash `59600163ccc9cffb8f3ae148beede7cd7866ae86` fica [nao verificado]. `app/docs/GRADE-MESTRA.md:140`, `app/docs/GRADE-MESTRA.md:297`, `app/docs/GRADE-MESTRA.md:548`, `app/docs/GRADE-MESTRA.md:596`, `app/docs/GRADE-MESTRA.md:663`.

**Morre?** indica o alcance da cura da nascente sobre a entrega da obra. **Ainda vivo?** se refere ao ramo identificado no código da cópia. Passivo, incidência atual, tempos, deploy e smoke ficam [nao verificado] quando exigem banco, logs ou execução. Apliquei os critérios de juiz e escritor de `CLAUDE.md:14`. Os caminhos das tabelas são relativos a `app/`. [nao verificado]

**Placar 20.** `J` é **um juiz por pergunta**, `E` é **um escritor por entidade** e `P` é **parâmetro consumido ou sem efeito**. A referência é a célula declarada da matriz. Onde a célula cobre a pergunta-mãe, a nota identifica a lacuna específica do leitor ou da porta. `core/contratos_estruturais.py:55`, `core/contratos_estruturais.py:90`, `core/contratos_estruturais.py:1038`.

## Obras 1–54

| ID | Nascente | Sítio | Morre? | Ainda vivo? | Placar 20 | Nota |
|---|---|---|---|---|---|---|
| FERIADO-ESCALA | padrão de feriado ao vincular | `escala/models.py:905` + `colaboradores/services/vinculo.py:297` | parcial | não no vínculo novo | fora do placar | A porta usa o padrão por cargo, posto e tipo. Ele não reclassifica vínculos antigos. `escala/models.py:908` |
| HJ-092 | independente | `chamados/services/materializacao.py:539`, `:595`, `:603` | não | sim para o ramo de divergência | fora do placar | A entrega é encaminhar a decisão humana. O materializador já distingue marco cobrado e marco da ata. `docs/BACKLOG.md:797` |
| LANCAR-AUSENCIA-AFASTA-DESLIGADO | independente | `ponto/views.py:2542`, `:2569` + `colaboradores/services/colaborador.py:56` | não | não no POST lido | fora do placar | Guarda de elegibilidade. A situação é validada pela porta antes da escrita. |
| PILHA-DA-BATIDA-RECALCULO-INLINE | independente | `ponto/registro_batida.py:197` + `ponto/services/fechamento.py:1831`, `:1848` | não | sim | fora do placar | O recálculo pode ocorrer no caminho da requisição. A latência fica [nao verificado]. |
| O-BITEMPORAL-2 | independente | `docs/BACKLOG.md:814` + `escala/models.py:1473` | não | não sei [nao verificado] | fora do placar | Entrega transversal de histórico de vigência e de registro. A obra usa 20/20 como pré-requisito. |
| IA-DOMINIO-POR-FERRAMENTA | permissão por ferramenta sem juiz aplicado | `core/ia_acoes.py:22`, `:53`, `:69` + `core/views_ia.py:859` | sim | sim | fora do placar | O gate de entrada usa a união das ações. Falta aplicar a permissão da ferramenta à identidade que pede a resposta. |
| REJULGAR-COMP10-4-REVERTIDOS | independente | `docs/BACKLOG.md:858` | não | não sei [nao verificado] | fora do placar | Ato sobre o passivo e comparação de resultados. A causa residual fica [nao verificado]. |
| TROCA-DE-POSTO-SEM-VINCULO | troca de posto sem porta única do ato | `colaboradores/views.py:337`, `:1415` + `escala/alimentacao.py:228` | sim | sim | fora do placar | Os caminhos de posto alteram o cadastro. A autoridade do vínculo do dia continua separada. O escritor de EscalaColaborador é coberto por `core/contratos_estruturais.py:139`. |
| LAVRAR-HE-PENDENTE-ESCALA | independente | `ponto/services/fechamento.py:152`, `:293` + `escala/models.py:1109`, `:1221` | não | não para a recarga corrigida | fora do placar | Reuso da alimentação no cálculo. Custo dos leitores restantes [nao verificado]. |
| N1-ALIMENTACAO-UNICA | independente | `folha/fora_da_folha.py:62` + `chamados/services/vigia_fio.py:600`, `:618` | não | não sei [nao verificado] | fora do placar | Obra de carga compartilhada entre leitores. `docs/BACKLOG.md:890` |
| BO-ADMIN-2710 | independente | `docs/BACKLOG.md:893` | não | não sei [nao verificado] | fora do placar | Triagem histórica. A linha remete cada ocorrência à obra responsável. |
| TRILHA-DO-VINCULO-MENTE | trilha de edição do vínculo sem dono do ato | `colaboradores/services/vinculo.py:307`, `:356`, `:381` | parcial | não no núcleo | E escala · `core/contratos_estruturais.py:139` | A porta distingue criação de edição e registra as fotos. O residual RT-03 fica [nao verificado]. `docs/BACKLOG.md:894` |
| REGISTRADO-NAO-E-VIVO | independente | `chamados/catalogo/motor.py:24` + `chamados/services/painel.py:316` | não | não na lista padrão | fora do placar | A lista padrão distingue cobrança e histórico. O complemento é de fiscalização. `docs/BACKLOG.md:896` |
| SILENCIO-VENCIDO-VOLTA | fila de cobrança com segundo juiz | `chamados/services/painel.py:189`, `:316`, `:331` + `chamados/services/verbos.py:47` | sim | sim | fora do placar | `qs_na_fila` volta a incluir o silêncio vencido. A lista ainda o exclui como histórico. |
| FECHAR3-SOBREVIVENTE-NO-RECONCILIAR | sobrevivente do dedup decidido por ordem | `chamados/models.py:807`, `:827` + `chamados/juizes.py:2352` | sim | não | J chamado, C8 · `core/contratos_estruturais.py:262` | A porta protege o sobrevivente que o juiz designa. `core/juizes.py:300` |
| V-FASE-AS-OF-TRANSFERIDO | empresa do dia de fase lida pelo cadastro atual | `ponto/management/commands/e6_oraculo.py:178` + `escala/services/fase_12x36.py:113`, `:146` + `escala/alimentacao.py:228` | sim | sim | J escala, vínculo do dia · `core/contratos_estruturais.py:444` | A busca histórica recebe a empresa atual. A cobertura `as of` da lavra, por si, fica fora da célula do placar. |
| V-FASE-AS-OF-PARIDADE-UNIVERSO | previsão do censo de fase em dois universos | `ponto/management/commands/censo_fase_12x36.py:176`, `:203`, `:212` | sim | sim | J célula/precedência, previsto · `core/contratos_estruturais.py:165` | O cenário atual lê célula e folga. O proposto deriva do template. A paridade hipotética fica fora do selo específico. |
| V-FASE-AS-OF-CUSTO | independente | `escala/services/fase_12x36.py:97`, `:109`, `:132` | não | sim para a busca | fora do placar | Procura lavras até cobrir o universo ou esgotar o iterador. O custo efetivo fica [nao verificado]. |
| DEGRAU-HE-100-CADASTRO | degrau de HE100 reproduzido | `ponto/calculador/periodo.py:173`, `:376` + `ponto/calculador/regras.py:825` + `ponto/motor_calculo_v2.py:3270`, `:3405` | parcial | sim | fora do placar | O limiar 120 aparece em mais de um cálculo. O novo cadastro é parte própria da entrega. `docs/BACKLOG.md:1021` |
| V-FASE-AS-OF-SEM-CELULA | dia sem célula não tem carimbo de fato novo | `escala/services/fase_12x36.py:174`, `:193` + `ponto/services/cartorio.py:2078`, `:2095` | sim | sim | fora do placar | A reabertura depende de carimbos das células existentes. |
| V-FASE-AS-OF-ATRASO | independente | `config/crons.py:616`, `:637` + `escala/services/fase_12x36.py:188` | não | sim na dependência do cron | fora do placar | Coordenação e frescor. Falta dependência direta da lavra do cartório. O atraso efetivo fica [nao verificado]. |
| V-FASE-AS-OF-SOBREPOSTA | cobertura de lavra de fase em duas chaves | `escala/services/fase_12x36.py:113`, `:137`, `:177`, `:193` | sim | sim | fora do placar | A leitura reconhece cobertura por dia. A reabertura procura a tripla exata da janela. |
| V-FASE-AS-OF-INTRADIA | lavra de fase sem escritor de versões intradia | `escala/services/fase_12x36.py:85`, `:91`, `:107` | sim | sim | fora do placar | A escrita substitui o snapshot pela chave `data_ref` no mesmo dia. |
| PROPOSITOR-JANELA-LONGA | fase histórica lida da última lavra | `escala/services/propositor.py:416`, `:490` + `escala/services/fase_12x36.py:117`, `:197` | sim | sim | fora do placar | O propositor confronta uma janela arbitrária com os itens da última lavra. |
| FILA-POR-CAUSA-AS-OF | fase histórica lida da última lavra | `chamados/services/fila_por_causa.py:184`, `:214`, `:230` + `escala/services/fase_12x36.py:117`, `:197` | sim | sim | fora do placar | A fila agrupa pela identidade da última lavra. A autoridade `as of` responde por dia. |
| GRADE-09-PAUTA | independente | `docs/BACKLOG.md:1072` + `ponto/services/cartorio.py:2078` | não | não sei [nao verificado] | fora do placar | Medida e entrega ao DP da diferença do passivo. |
| NOREVERSE-UI-NO-CORE | independente | `pautas/services.py:416`, `:436` | não | não no sítio lido | fora do placar | A ponte de tela resolve a URL fora do ramo que já gravou. A verificação da corrente fica [nao verificado]. `docs/BACKLOG.md:1085` |
| ISENTO-FLAG-ATEMPORAL | isenção por dia sem autoridade temporal | `colaboradores/models.py:294` + `core/constants.py:13` + `ponto/services/cartorio.py:567`, `:1219`, `:1438` | sim | sim | fora do placar | A flag atual entra na impressão de dia antigo. O efeito no passivo fica [nao verificado]. |
| GRADE-FOLGA-C5 | folga lançada fora do juiz | `chamados/management/commands/auditar_invariantes_chamados.py:1559` + `ponto/precedencia.py:212`, `:222` | sim | sim | J célula/precedência · `core/contratos_estruturais.py:165` | A auditoria usa qualquer vínculo que cubra a data. O juiz distingue calendário do vínculo e folga pessoal. `core/juizes.py:212` |
| GRADE-FOLGA-GERADOR | folga lançada fora do juiz | `escala/models.py:1263`, `:1297`, `:1321` + `ponto/precedencia.py:212`, `:222` | sim | sim | J célula/precedência · `core/contratos_estruturais.py:165` | O gerador lê a foto do próprio vínculo e pode deixar de ver uma folga pessoal lançada no anterior. |
| P14-CONFIRMAR-IMPLAUSIVEL-SEM-MOTIVO | motivo de confirmação implausível sem guarda na porta | `chamados/services/validacao.py:697`, `:773`, `:787` + `chamados/services/declaracao_texto.py:738` | sim | sim | E chamado, lacuna na guarda do ato · `core/contratos_estruturais.py:496` | A confirmação booleana permite o ato sem exigir o motivo escrito. |
| PERGUNTA-VIVA-LOGA-PREVISTO-COMO-FALHA | independente | `chamados/juizes.py:360`, `:411` + `chamados/services/materializacao.py:595` | não | sim | fora do placar | Divergência prevista registrada como erro. Volume atual [nao verificado]. |
| JUIZ-UNICO-DA-ANCORA | âncora sugerida 12x36 sem juiz único | `escala/services/fase_12x36.py:27` + `escala/services/fase_na_tela.py:50` + `ponto/management/commands/censo_fase_12x36.py:203` + `escala/services/propositor.py:308`, `:333` | sim | sim | fora do placar | Censo, tela e propositor têm derivações próprias. O contrato da escala registra duas perguntas de vínculo. `core/juizes.py:1334`, `core/contratos_estruturais.py:444` |
| BO-[nome]-DESTRAVA | dono de cadastro não chega ao destrava da folha | `ponto/management/commands/e6_oraculo.py:53`, `:98` + `folha/fora_da_folha.py:275` + `folha/retrato_folha.py:1079` | parcial | sim para a lacuna da projeção | fora do placar | A projeção da porta não recebe o dono de cadastro do dia. Texto e caminho de tela fazem parte da obra. `docs/BACKLOG.md:1135` |
| FIO-DO-ADMIN-LAVRA-VELHA | independente | `chamados/services/vigia_fio.py:273` + `escala/services/cadastro_realidade.py:430` + `ponto/services/teto_na_lista.py:120` | não | sim | fora do placar | Falta de aviso de frescor nesses leitores. Incidência atual [nao verificado]. |
| AVISO-ATIVO-SEM-VINCULO-NO-DIA | aviso consulta existência de escala em vez do vínculo do dia | `inteligencia/management/commands/gerar_alertas.py:113` + `ponto/supra_juiz.py:196` + `escala/alimentacao.py:228` | parcial | sim no aviso lido | J escala, vínculo do dia · `core/contratos_estruturais.py:444` | A consulta de existência pode responder quando nenhuma escala cobre aquele dia. Emitir o aviso é parte própria da entrega. |
| REABRIR-DISPUTA-ADMIN-IGNORA-RETORNO | independente | `chamados/services/acoes_disputa.py:299`, `:340` + `chamados/models.py:1453` | não | sim | fora do placar | O chamador pode ignorar a recusa do escritor e emitir sucesso. |
| CXR-FALHA-CALADA | independente | `escala/services/cadastro_realidade.py:968`, `:1014`, `:1027` | não | sim | fora do placar | Alguns braços registram exceção e seguem sem indicar indisponibilidade na lista. |
| COL309-SETEMBRO | independente | `ponto/portas/celula.py:112`, `:214` + `docs/BACKLOG.md:1159` | não | não sei [nao verificado] | fora do placar | Ato nominal de regeneração por uma porta existente. O recálculo apareceu como complemento separado. |
| CAL-CUSTO-NO-MAPA-DE-FUROS | independente | `relatorios/furos.py:127`, `:141` | não | sim para a leitura por colaborador | fora do placar | Há leitura de grade por colaborador elegível. O custo fica [nao verificado]. |
| ENTRADA-BLOQUEADA-SEM-DATA-TURNO | nascimento de chamado sem dia do fato | `ponto/views.py:294`, `:320` + `chamados/catalogo/modulos.py:182`, `:219` | sim | sim | J chamado, C5 · `core/contratos_estruturais.py:262` | O contexto de nascimento carrega pendências e total. Falta o `data_turno` que o juiz procura. |
| POSICAO-FALTAS-E-AUSENCIAS | posição tipada de faltas e ausências sem juiz | `folha/export.py:169` + `ponto/turnos.py:2560`, `:2816` | sim | não sei [nao verificado] | fora do placar | Existem autoridades separadas de falta e cobertura. A composição pedida não apareceu nos sítios lidos. `docs/BACKLOG.md:1182` |
| POSICAO-FERIAS-ATE-A-DATA | férias proporcionais por data sem juiz | `ferias/situacao.py:138`, `:167` + `ferias/models.py:108` | sim | sim na interface lida | fora do placar | A consulta recebe o dia de referência e lista períodos. Falta apurar os avos até esse dia. `docs/BACKLOG.md:1183` |
| BO2910-ESTRUTURA | troca de posto sem porta única do ato + outras classes [nao verificado] | `colaboradores/views.py:337`, `:1415` + `chamados/reconciliador.py:335` + `ponto/services/ausencia.py:1099` + `colaboradores/services/desligamento.py:235` + `ponto/portas/celula.py:1015` | parcial | sim para o posto | fora do placar para o posto | O item 01 compartilha a origem. Os demais tratam módulo de chamado, período de ausência, vigência e veredito humano. A atribuição de cada ocorrência fica [nao verificado]. `docs/BACKLOG.md:1191` |
| GRADE-DEPENDE-DA-JANELA | montagem da grade decide o dia pela janela | `escala/utils.py:1322`, `:1384`, `:1824`, `:2138` + `ponto/services/cartorio.py:1202` | sim | sim para a dependência do insumo | J célula/precedência e J turno/marcos · `core/contratos_estruturais.py:165`, `:225` | A alocação de batidas depende dos limites da chamada. Reprodução do caso nominal e selo específico de invariância [nao verificado]. `docs/BACKLOG.md:1200` |
| PAUTAS-PREFILTRO-REPETIDO | independente | `ponto/management/commands/pautas_discordantes.py:99`, `:118` + `ponto/management/commands/e6_oraculo.py:182` | não | sim | fora do placar | As duas chamadas reutilizam o juiz do dono. A repetição é de trabalho. |
| OUTBOX-EVENTO-DUPLO | independente | `ponto/portas/celula.py:981`, `:1002` + `chamados/management/commands/consumir_eventos_celula.py:1` | não | não sei [nao verificado] | fora do placar | A porta emite por chamada. O chamador que a acionaria duas vezes no mesmo ato fica [nao verificado]. `docs/BACKLOG.md:1203` |
| JUSTIFICATIVA-ENCERRA-ORIGEM | fechamento da origem fora do ato da aprovação | `ponto/services/justificativa.py:16`, `:65`, `:86`, `:217` | parcial | não na porta lida | E chamado · `core/contratos_estruturais.py:496` | Aprovação chama a retratação no mesmo ato e conserva a guarda do chamado irmão. Retenções nominais [nao verificado]. |
| BO-ATESTADO-ANDROID-ERRO-AO-ENVIAR | independente | `api/views.py:1824`, `:1857` + `docs/BACKLOG.md:1223` | não | não para reenvio idêntico no servidor | fora do placar | Reenvio idêntico devolve a linha existente. Veto real tem ramo próprio. Comportamento do Android [nao verificado]. |
| REABRIR-NAO-DESTROI | reabertura sem escritor do fato anterior | `chamados/services/disputa_emissao.py:1628`, `:1659`, `:1695` | sim | não no caminho lido | E chamado · `core/contratos_estruturais.py:496` | A decisão registra a foto da resposta antes de descarimbar. |
| HE-DE-14H-NUM-DIA (col945 12/09) | teto do dia ignorado no pareamento | `ponto/turnos.py:66`, `:1427`, `:1456` + `docs/BACKLOG.md:1235` | parcial | não no ramo lido | J turno/marcos · `core/contratos_estruturais.py:225` | O pareador lê o teto e pode deixar a saída órfã. O caso e o gravado antigo ficam [nao verificado]. |
| BUG-ANCORA-NAO-REGENERA | alteração de âncora sem efeito na célula | `escala/signals.py:87`, `:113`, `:134` + `ponto/portas/celula.py:112` | parcial | não no sinal lido | fora do placar | O sinal chama a porta de regeneração. O contrato da célula cobre o escritor e a obra inclui o alcance do disparo. `core/contratos_estruturais.py:113` |
| O-HAIKU-INVESTIGA | independente | `api/views_mensageria.py:1935`, `:1981`, `:2021` + `docs/BACKLOG.md:1262` | não | não sei [nao verificado] | fora do placar | Entrega de consultas e propostas. Os leitores correlatos existem. O pacote e o juiz incremental pedidos ficam [nao verificado]. |
| O-JANELA-HE | janela de HE sem regra única para as duas pontas | `ponto/janela_he.py:107`, `:150` + `ponto/motor_calculo_v2.py:1812`, `:1854` + `ponto/calculador/regras.py:30` | parcial | não nos caminhos lidos | P folha/export · `core/contratos_estruturais.py:307`, `:320` | Motor e calculador leem a janela. Cadastro, ato administrativo e tela são entregas adicionais. |

## Obras 55–102

| ID | Nascente | Sítio | Morre? | Ainda vivo? | Placar 20 | Nota |
|---|---|---|---|---|---|---|
| CHAMADO-NASCE-EM-EXPORTADA | independente | `chamados/models.py:1062`, `:1092` + `folha/export.py:565` | não | não como defeito descrito | fora do placar | O gate vigente do nascimento usa tranca. A exportação vigente é outra pergunta. `docs/BACKLOG.md:1269` |
| COL369-TRES-RESTANTES | [nao verificado] | `ponto/turnos.py:502`, `:1565` + `ponto/services/dia_pago.py:364`, `:481` + `colaboradores/services/vinculo.py:550`, `:567` | não sei [nao verificado] | não sei [nao verificado] | [nao verificado] | São três ocorrências. Os sítios tratam pausa, campos gravados e guarda da vigência, sem reproduzir a atribuição nominal. `docs/BACKLOG.md:1288` |
| O-GEO-DECISAO | ato de geofence dividido entre autorizar e recusar | `chamados/services/fio.py:669`, `:682` + `chamados/services/acoes_chamado.py:171`, `:251`, `:277` + `ponto/presenca.py:29` | parcial | sim | fora do placar | Os ramos do ato são separados. A notificação de ausência de ping é outro componente. A escrita de Batida já tem seu contrato. `core/contratos_estruturais.py:91` |
| O146 | independente | `escala/models.py:78`, `:83` + `ponto/management/commands/gerar_celulas.py:128` + `ponto/motor_calculo_v2.py:1457` | não | não no motor | fora do placar | O motor consome a extra declarada no DNA. A linha do copiloto fica [nao verificado]. |
| O236 | independente | `folha/export.py:1185`, `:1221`, `:1240` + `docs/BACKLOG.md:1337` | não | não sei [nao verificado] | fora do placar | Censo por motivo de retenção. O export já tem autoridade para a classificação. |
| O121 | guarda do passado do vínculo concorrendo com a guarda da célula | `colaboradores/services/vinculo.py:69`, `:72`, `:330` + `ponto/portas/celula.py:218` + `folha/export.py:607` | parcial | sim no caminho geral | J folha/export, K9 · `core/contratos_estruturais.py:353` | A edição comum ainda passa pelo piso e pela guarda de TXT ou holerite. O piso S99 e o wizard têm escopo próprio. `docs/BACKLOG.md:1362` |
| O123 | independente | `colaboradores/views.py:3045` + `chamados/relatorios_disputa.py:47`, `:53` | não | não para a carga proposta | fora do placar | O coletor carrega perguntas, célula, destinos e decisões em lote. O custo residual fica [nao verificado]. |
| O118 | marcos do dia reconstruídos com uma só pausa | `escala/utils.py:27`, `:65`, `:1458`, `:1664` | parcial | não no ramo lido | J turno/marcos, T5 · `core/contratos_estruturais.py:225` | O alinhamento recebe todas as pausas do DNA. A prova de tela fica [nao verificado]

[truncado em 20000 de 75137 caracteres]

## 27/11 15:15 · sessao 878f619f0872 · INVENTARIO-PELA-REGRA · leitura · origem=Code

Conferi o HEAD `59600163ccc9cffb8f3ae148beede7cd7866ae86` e comparei os arquivos citados com os blobs desse commit. A sonda está abaixo. Os números da comp 11 e o cadastro atual da Sombra ficam **[nao verificado]** até a execução pelo envelope. [bin/codex_pergunta.sh:88](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/codex_pergunta.sh:88) [bin/codex_pergunta.sh:611](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/codex_pergunta.sh:611)

## A. R4

Para cada `Empresa` de PK 2, 3 e 4, a entrada de leitura é `folha.porta_export.medir(empresa, 11, 2026)`. A janela vem de `janela_fechamento(11, 2026, empresa)`. A sonda confere o intervalo de **21/10 a 20/11/2026** e imprime o marco lido do banco para cada empresa. [app/core/placar_estrutural.py:214](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/placar_estrutural.py:214) [app/folha/porta_export.py:319](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:319) [app/ponto/janelas.py:54](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/janelas.py:54) [app/ponto/services/fechamento.py:1577](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1577)

| Par | Função que mede |
|---|---|
| Tela × PDF | `medir` compara `espelho_do_colab(c, hoje, ini, fim, agora=ref)` e `_coletar_dados_espelho_mes(c, 11, 2026, agora=ref)`. Retorno `tela_x_pdf`. [app/folha/porta_export.py:482](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:482) [app/folha/porta_export.py:502](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:502) |
| Cartão × TXT | `relatorios.cartao_pela_celula.divergencia(c, 11, 2026, resumo)`, chamado por `cartao_x_txt_divergentes`. O retorno de `medir` é da frota da competência. Para obter os números por empresa, a sonda cruza os IDs divergentes emitidos pelo comando com `Colaborador.empresa_id`. [app/relatorios/cartao_pela_celula.py:216](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/relatorios/cartao_pela_celula.py:216) [app/relatorios/management/commands/cartao_x_txt_divergentes.py:68](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/relatorios/management/commands/cartao_x_txt_divergentes.py:68) [app/folha/porta_export.py:732](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:732) |
| Espelho × DiaPago | `dias_fora_do_dia_pago(c, 11, 2026, ini, fim, versao='motor', dp_por_dia=None)`. Retornos `espelho_x_dia_pago` e `espelho_x_dia_pago_nao_medido`. [app/folha/porta_export.py:26](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:26) [app/folha/porta_export.py:603](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:603) |
| Fechamento × soma do DiaPago | `_medir_dia_pago(c, fech, badge=None)`, interna a `medir`, usa `soma_da_competencia(c, 11, 2026, versao='motor')`. Retorno `fechamento_x_soma_dias_pagos`. O universo inclui quem tem fechamento ou lavratura, inclusive fora do TXT. [app/folha/porta_export.py:384](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:384) [app/folha/porta_export.py:457](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:457) [app/folha/porta_export.py:653](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:653) [app/ponto/services/dia_pago.py:550](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/dia_pago.py:550) |
| Topo × soma das linhas | `medir` compara `total_relogio` com a soma de `minutos_realizados` dos dias da janela. Retorno `topo_x_coluna`. [app/folha/porta_export.py:515](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:515) |
| App × tela | `AppLeATelaTest.test_MORDE_o_endpoint_nao_remonta_a_palavra_do_dia()` e `test_MORDE_o_endpoint_LE_o_espelho()`. É um selo AST do código, aplicável às três empresas. [app/api/tests/test_r4_par6_app_le_a_tela.py:53](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/api/tests/test_r4_par6_app_le_a_tela.py:53) |
| Linhas do DiaPago × badge | `linha_x_badge(c, 11, 2026, badge, soma=None)`, com `badge` igual ao resumo do espelho. Mede noturnas, atraso e trabalhadas mais folga quando a fonte do badge é `folha`. Os retornos distinguem divergência, exportada e motivo de ausência de medição. [app/folha/porta_export.py:251](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:251) [app/folha/porta_export.py:449](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:449) [app/folha/porta_export.py:717](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:717) |

`medir` é a entrada de leitura. O CLI `selo_leitores_no_mesmo_numero` vigia e assume tentativas, depois chama `concluir`, que grava o carimbo por `lavrar` em `MetricaSnapshot.objects.update_or_create`. A chave `calendario_x_espelho` é de compatibilidade e vem sempre vazia. [app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:76](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:76) [app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:197](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:197) [app/folha/porta_export.py:1279](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:1279) [app/folha/porta_export.py:785](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:785) [app/folha/porta_export.py:608](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:608)

## B. R5

**R5 so com escrita: fica fora (proibido medida nova)**. O `.sh` chama o Python fixado em **10/2026**. Cada passada aplica o cartório e grava o recálculo. A prova compara a foto após a primeira escrita com a foto após a segunda. O dry do cartório, sem `--apply`, julga a fila `data < hoje`. Duas leituras dessa fila observam o mesmo estado e deixam a transição exigida pelo R5 sem prova. [bin/r5_idempotencia_frota.sh:24](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/r5_idempotencia_frota.sh:24) [bin/r5_idempotencia_frota.py:26](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/r5_idempotencia_frota.py:26) [bin/r5_idempotencia_frota.py:104](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/r5_idempotencia_frota.py:104) [bin/r5_idempotencia_frota.py:124](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/r5_idempotencia_frota.py:124) [app/ponto/management/commands/processar_cartorio.py:25](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/processar_cartorio.py:25) [app/ponto/services/cartorio.py:1905](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/cartorio.py:1905) [app/ponto/services/fechamento.py:1168](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1168)

## C. Becos, órfãs e dias sem caminho

- **Beco de disputa.** `chamados.services.becos.classificar(disputa)` devolve `fechavel`, `barrada` ou `None`. O cron usa `--alarme`, com RC 2 se houver fecháveis. Até em dry o comando executa `get_user_sistema()`, que pode fazer `INSERT` via `get_or_create`. A sonda chama diretamente o classificador e usa `dia_do_fato(chamado)` para o recorte. Becos sem dia apurável aparecem em linha própria. [app/config/crons.py:663](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:663) [app/chamados/management/commands/fechar_becos_disputa.py:27](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/fechar_becos_disputa.py:27) [app/core/utils.py:18](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/utils.py:18) [app/chamados/services/becos.py:25](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/becos.py:25) [app/chamados/juizes.py:89](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/juizes.py:89)
- **Dia retido sem caminho.** `censo_alcance(empresas=[empresa], hoje=hoje, agora=ref, limite=0)` é o leitor do `vigiar_fio_morto` que devolve todos os `sem_porta`, mesmo com limite zero. A sonda conta por causa, detalhe e papel em `11/2026` e indica quando a competência está fora do universo do vigia. A execução de frota do CLI grava carimbo em disco. [app/config/crons.py:1746](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:1746) [app/chamados/services/vigia_fio.py:524](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/vigia_fio.py:524) [app/chamados/services/vigia_fio.py:622](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/vigia_fio.py:622) [app/chamados/management/commands/vigiar_fio_morto.py:131](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/vigiar_fio_morto.py:131)
- **Fio mudo e emissão que faltou.** `contador_fio_mudo(hoje=hoje, agora=ref, ini=ini, fim=fim, empresas=[eid])` mede motivos devidos sem pergunta viva. `motivos_que_o_evento_deixou(cel, hoje, ref, memo=memo)` mede o que faltou emitir na célula. Ambos são leitores. O `--alarme` do cron dá RC 2 pelo segundo contador. [app/config/crons.py:1548](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:1548) [app/chamados/management/commands/disparar_perguntas_competencia.py:77](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/disparar_perguntas_competencia.py:77) [app/chamados/management/commands/disparar_perguntas_competencia.py:221](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/disparar_perguntas_competencia.py:221) [app/chamados/services/fabrica_da_celula.py:234](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/fabrica_da_celula.py:234)
- **Órfãs e RC.** `orfas_pela_lampada_sem_evento(agora=ref, colaborador_ids=ids)` devolve os IDs que o evento devia ter encerrado. `reconciliar_perguntas_orfas` é leitura e `--alarme` dá RC 2 se houver achado. No `reconciliar_chamados`, `perguntas_orfas_reatavies(colab)` devolve um queryset do universo elegível, que fica fora da soma de alarme do RC. A sonda recorta os dois por `data_evento`. [app/config/crons.py:1551](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/crons.py:1551) [app/chamados/reconciliador.py:1325](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/reconciliador.py:1325) [app/ponto/management/commands/reconciliar_perguntas_orfas.py:24](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/reconciliar_perguntas_orfas.py:24) [app/chamados/reconciliador.py:1640](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/reconciliador.py:1640) [app/chamados/management/commands/reconciliar_chamados.py:97](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/management/commands/reconciliar_chamados.py:97)
- **E6.** `e6_oraculo --mes 11 --ano 2026 --hoje 2026-11-27` é leitura sem `--csv`. Mede divergência do espelho por dia-colaborador, com dono atribuído por `InsumosDoDono.dono(dia)`. O comando percorre a frota ativa com fechamento. A sonda usa as colunas `emp`, `dono_da_divergencia` e `classe` do CSV oficial em memória para imprimir o resultado por empresa. Se o comando reportar erro de espelho, a sonda levanta exceção. [app/ponto/management/commands/e6_oraculo.py:182](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/e6_oraculo.py:182) [app/ponto/management/commands/e6_oraculo.py:276](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/e6_oraculo.py:276) [app/ponto/management/commands/e6_oraculo.py:303](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/e6_oraculo.py:303) [app/ponto/management/commands/e6_oraculo.py:442](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/e6_oraculo.py:442)

## D. Layout do TXT e cadastro L-099

A view obtém os objetos com `recalcular_fechamento_mes(11, 2026, empresa_id=e.pk, somente_leitura=True, retornar_objeto=True)`. Passa esses mesmos objetos a `classificar_export` e a `montar_linhas_export(empresa, mes, ano, itens=itens, fechamentos=fechs)`. O primeiro valor devolvido pela montagem é `linhas`, que entra em `validar_layout_dominio(linhas)`. A aprovação em lote e o registro de `ExportacaoDominio` ficam no caminho da view. [app/folha/views.py:214](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:214) [app/folha/views.py:253](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:253) [app/folha/views.py:275](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:275) [app/folha/views.py:282](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:282) [app/folha/views.py:304](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:304)

A sonda avalia todo o universo da competência e valida o TXT montado para quem a regra admite. Quando há erro, remonta os blocos por colaborador com a mesma função, preserva o bloco da rubrica `8792` com seus registros `11` e confere que linhas e erros recompostos coincidem com os da empresa. Cada achado fica em **CADASTRO L-099**, com `colaborador_id`, regra e campo. A mensagem bruta do validador pode conter a linha inteira e fica em memória. [CLAUDE.md:183](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/CLAUDE.md:183) [app/folha/export.py:266](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:266) [app/folha/export.py:699](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:699) [app/folha/export.py:725](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:725)

## E. Feriado municipal da comp 12

A data localizada é **10/12/2026, Aniversário de Londrina/PR**. O teste cadastra esse feriado e uma leitura anterior da Sombra registra 10/12 como feriado por posto na comp 12. A presença no cadastro atual da Sombra está **[nao verificado]**. [app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:39](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:39) [app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:62](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:62) [lista anterior da Sombra](/home/ronald/saas-hasner/logs/FERIADO-POR-POSTO/pouso_lista_v2/lista.out:50)

O cadastro é `core.models.Feriado`, tabela `core_feriado`. A consulta abaixo filtra `abrangencia='municipal'` e `data__range=(21/11/2026, 20/12/2026)` e ordena por UF, município e data. Por empresa, o cruzamento é com `Posto.praca` e `Posto.municipio`, usando `core.feriados.alcanca` para aplicar a regra de UF e município. [app/core/models.py:5](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/models.py:5) [bin/sombra_regras_pessoais.py:194](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sombra_regras_pessoais.py:194) [app/colaboradores/models.py:230](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/colaboradores/models.py:230) [app/core/feriados.py:57](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:57)

## F. Sonda Python

Para `bin/codex_pergunta.sh --sonda`, o caminho em `CODEX_SONDA_ENVELOPE` é o **adaptador de Sombra existente**, `/home/ronald/saas-hasner/logs/FILA-CODEX-1811/sondas/envelope_sombra.sh`. O padrão do wrapper passa só o arquivo Python e cai em produção. `SONDA_ARVORE` deve apontar para o `app/` da cópia conferida, pois é esse caminho que o envelope monta no contêiner. O envelope prepara role, grants e arquivos próprios, por isso esta entrega ficou em leitura de código e checagem em memória. [bin/codex_pergunta.sh:623](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/codex_pergunta.sh:623) [adaptador da Sombra](/home/ronald/saas-hasner/logs/FILA-CODEX-1811/sondas/envelope_sombra.sh:3) [bin/sonda_leitura.sh:93](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:93) [bin/sonda_leitura.sh:126](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:126) [bin/sonda_leitura.sh:153](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:153) [bin/sonda_leitura.sh:232](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:232)

Fixei o relógio da medição em **27/11/2026 às 15:15, São Paulo**. O cabeçalho imprime também a hora real de execução. O `raise` final provoca rollback, e só essa sentinela é suprimida depois da saída do `atomic()`, para o wrapper aceitar o stdout. A sintaxe e exemplos dos rótulos do layout foram checados em memória. A execução Django permanece **[nao verificado]**. [app/ponto/services/fechamento.py:634](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:634) [app/chamados/tests/cef_c3_sonda.py:22](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/cef_c3_sonda.py:22) [bin/codex_pergunta.sh:629](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/codex_pergunta.sh:629)

```python
import logging
import re
import sys
from collections import Counter, defaultdict
from contextlib import contextmanager, nullcontext, suppress
from csv import DictReader
from datetime import date, datetime, timezone as utc
from io import StringIO
from unittest.mock import patch
from zoneinfo import ZoneInfo

from django.contrib.auth.models import User
from django.core.management import call_command
from django.db import connection, transaction
from django.db.models import Exists, OuterRef
from django.utils import timezone

from api.tests.test_r4_par6_app_le_a_tela import AppLeATelaTest
from chamados.juizes import dia_do_fato, tranca_em_lote
from chamados.management.commands.disparar_perguntas_competencia import contador_fio_mudo
from chamados.models import DisputaSupervisao, PerguntaDisputa
from chamados.reconciliador import orfas_pela_lampada_sem_evento, perguntas_orfas_reatavies
from chamados.services import becos
from chamados.services.fabrica_da_celula import motivos_que_o_evento_deixou
from chamados.services.vigia_fio import censo_alcance
from colaboradores.models import Colaborador, Empresa, Posto
from core.feriados import alcanca
from core.models import Feriado
from escala.models import CelulaDia
from folha.export import classificar_export, montar_linhas_export, validar_layout_dominio
from folha.porta_export import medir
from ponto.janelas import janela_fechamento
from ponto.management.commands import e6_oraculo
from ponto.services.fechamento import (
    marco_da_competencia, recalcular_fechamento_mes, universo_da_competencia,
)

HEAD = "59600163ccc9cffb8f3ae148beede7cd7866ae86"
EMPRESAS = (2, 3, 4)
MES, ANO = 11, 2026
INI, FIM = date(2026, 10, 21), date(2026, 11, 20)
JANELA_12 = (date(2026, 11, 21), date(2026, 12, 20))
FUSO = ZoneInfo("America/Sao_Paulo")
REF = datetime(2026, 11, 27, 15, 15, tzinfo=FUSO)
HOJE = REF.date()


class F

[truncado em 20000 de 31363 caracteres]

## 27/11 15:11 · sessao 05e8003882ed · INVENTARIO-PELA-REGRA · construtor · origem=Code

Corrigi as cinco medições nos três módulos da bateria. A sintaxe dos módulos foi verificada. O recorte Django e os mutantes na pista do Code ficam [nao verificado].

### Diff por caso

- **ESC-11** cobra 630 min trabalhados e 150 min de extra total, com 120 em HE50 e 30 em HE100 de dia normal. O excedente é `HE total − teto = 30`, conforme o corte HE100-RUBRICA. [test_inventario_escala.py:327](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:327)
- **AUS-05** deixa o modelo derivar `instancia_marco` para `batida_ausente`. A pergunta segue aberta, com 480 min previstos, furo apurado e zero falta. [test_inventario_ausencia.py:103](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:103)
- **FOL-03** mapeia e homologa `9902` em `folga_trabalhada_diurna`. Confere os 240 min no fechamento antes de comparar o delta do TXT, que continua exigindo zero em HE50 e HE100 de feriado. [test_inventario_folha.py:56](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:56) · [test_inventario_folha.py:164](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:164)
- **FOL-13** usa 60 min de HE50 autorizada com rubrica homologada. Faz dois POSTs idênticos, exige dois downloads 200, confere a presença dos 60 min e compara bytes e totais por rubrica. [test_inventario_folha.py:53](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:53) · [test_inventario_folha.py:309](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:309)

**ESC-12, AUS-03 e FOL-14** estão idênticos à base. [test_inventario_escala.py:335](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:335) · [test_inventario_ausencia.py:61](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:61) · [test_inventario_folha.py:330](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:330)

### Recorte para o Code

Da raiz de `app`, na pista de teste do Code. O settings `ci` lê `DB_HOST` e `REGUA_DB` da pista. [config/settings/ci.py:6](/home/ronald/wt-codex-inventario-pela-regra-143921/app/config/settings/ci.py:6) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci --noinput \
  ponto.tests.test_inventario_escala \
  ponto.tests.test_inventario_ausencia \
  folha.tests.test_inventario_folha
```

### RED e mutantes

No RED isolado de **ESC-11**, as asserções aceitaram `120/30`. Transferir os 30 min para HE50 derrubou as duas asserções por faixa. Calcular `HE50 − teto` derrubou a do excedente com `0 != 30`. A execução integrada desses mutantes e dos demais fica [nao verificado]. [test_inventario_escala.py:328](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:328)

Os comandos abaixo aplicam uma substituição em memória por processo e rodam o caso correspondente. [ESC-11:327](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:327) · [AUS-05:103](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:103) · [FOL-03:56](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:56) · [FOL-13:309](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:309)

```bash
mutante() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 - "$@" <<'PY'
import importlib
import sys
from pathlib import Path

import django
from django.core.management import execute_from_command_line

rotulo, antes, depois = sys.argv[1:]
django.setup()
modulo = importlib.import_module(rotulo.rsplit('.', 2)[0])
arquivo = Path(modulo.__file__)
fonte = arquivo.read_text(encoding='utf-8')
assert fonte.count(antes) == 1, (arquivo, antes)
exec(compile(fonte.replace(antes, depois, 1), str(arquivo), 'exec'), vars(modulo))
execute_from_command_line(['manage.py', 'test', '--noinput', rotulo])
PY
}

esc11=ponto.tests.test_inventario_escala.InventarioEscalaTest.test_ESC_11_extra_integral_acima_de_duas_horas
aus05=ponto.tests.test_inventario_ausencia.InventarioAusenciaTest.test_AUS_05_uma_batida_com_pergunta_em_aberto
fol03=folha.tests.test_inventario_folha.InventarioFolhaTest.test_FOL_03_dsr_trabalhado_em_rubrica_propria
fol13=folha.tests.test_inventario_folha.InventarioFolhaTest.test_FOL_13_gerar_duas_vezes_mesmos_bytes_e_totais

# M1 — ESC-11: deslocar a HE100 para HE50, preservando o total.
mutante "$esc11" '        # HE100-RUBRICA' \
  $'        for periodo in resultado.periodos:\n            periodo.minutos_extra_50 += periodo.minutos_extra_100\n            periodo.minutos_extra_100 = 0\n        # HE100-RUBRICA'

# M2 — ESC-11: medir o excedente só pela HE50.
mutante "$esc11" 'extra_total - teto' 'resultado.horas_extra_50 * 60 - teto'

# M3 — AUS-05: forçar "fim" na pergunta genérica.
mutante "$aus05" $'            data_evento=data,\n        )' \
  $'            data_evento=data, instancia_marco=\'fim\',\n        )'

# M4 — FOL-03: voltar a mapear 9902 para o evento antigo.
mutante "$fol03" "'folga_trabalhada_diurna': DSR_TRABALHADO" \
  "'dsr_trabalhado_diurno': DSR_TRABALHADO"

# M5 — FOL-13: retirar da massa o evento exportável.
mutante "$fol13" \
  $'colab = self.massa_comercial({data: (\'08:00\', \'12:00\', \'13:00\', \'18:00\')})\n        self.autorizar(colab, data, 60)\n        self.fechar(colab, 11)' \
  $'colab = self.massa_comercial({data: COMERCIAL})\n        self.fechar(colab, 11)'

# M6 — FOL-13: alterar os bytes do segundo download após conferir o 200.
mutante "$fol13" '        self.assertEqual(primeiro.content, segundo.content)' \
  $'        segundo.content += b\'\\n\'\n        self.assertEqual(primeiro.content, segundo.content)'
```

## 27/11 15:10 · sessao 2e975728d169 · - · - · origem=Ronald (tmux)

Auditei os 40 itens de `RETRATO-DOIS-RELOGIOS` a `O320` e mais 22 de `EMPRESA-ID-ISDIGIT` ao fim do anexo. Enviei as linhas e os ajustes de consolidação ao agente principal. A cópia já tem o caminho da saída tardia de `O232` e a correção de pontualidade de `O73b`. [app/escala/utils.py:1999] [app/escala/tests/test_saida_valida_cross_meia_noite.py:80] [app/ponto/motor_calculo_v2.py:2822]

Estado de produção e identificação Git da cópia [nao verificado].

## 27/11 15:09 · sessao f553e73e9590 · - · - · origem=Ronald (tmux)

O HEAD de `/home/ronald/saas-hasner` é `59600163ccc9cffb8f3ae148beede7cd7866ae86`. Os arquivos A/B e os dois envelopes na cópia de leitura coincidem byte a byte com os blobs do commit. A resolução do hash pelo wrapper está em `bin/codex_pergunta.sh:116`.

**A.** Para cada `Empresa.objects.get(pk=eid)`, chame `folha.porta_export.medir(empresa, 11, 2026)`. A janela vem de `janela_fechamento(11, 2026, empresa)`, cujo resultado com corte 21 é `21/10/2026–20/11/2026`. O marco pode ser lido com `marco_da_competencia(eid, 11, 2026)`. `app/folha/porta_export.py:319`, `app/folha/porta_export.py:343`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:54`, `app/ponto/services/fechamento.py:1577`.

| Par | Função e resultado |
|---|---|
| Tela × PDF | `medir` compara `espelho_do_colab(c, hoje, ini, fim, agora=instante)` com `_coletar_dados_espelho_mes(c, 11, 2026, agora=instante)`. Chave `tela_x_pdf`. `app/folha/porta_export.py:482`, `app/folha/porta_export.py:502`, `app/folha/porta_export.py:699`. |
| Cartão × TXT | `medir` chama `cartao_x_txt_divergentes`. O comando mede todas as empresas ativas e chama `relatorios.cartao_pela_celula.divergencia(colab, 11, 2026, resumo)`. Para contagem por empresa, rode o comando com `ids=True` e `agora` ISO fixo, capture a saída em `StringIO` e associe os IDs `colN` à empresa pelo ORM. `app/folha/porta_export.py:670`, `app/folha/porta_export.py:732`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:20`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:68`, `app/relatorios/cartao_pela_celula.py:216`. |
| Espelho × DiaPago | `dias_fora_do_dia_pago(colab, 11, 2026, ini, fim, versao='motor', dp_por_dia=None)`, chamada por `medir`; chaves `espelho_x_dia_pago` e `espelho_x_dia_pago_nao_medido`. `app/folha/porta_export.py:26`, `app/folha/porta_export.py:603`, `app/folha/porta_export.py:703`. |
| Fechamento × soma do DiaPago | `_medir_dia_pago(c, fech, badge=None)`, interna a `medir`, usa `soma_da_competencia(c, 11, 2026, versao='motor')`. Chaves `fechamento_x_soma_dias_pagos` e `colabs_sem_lavratura`. `app/folha/porta_export.py:384`, `app/folha/porta_export.py:457`, `app/folha/porta_export.py:664`, `app/ponto/services/dia_pago.py:550`. |
| Topo × soma das linhas | `medir` compara `total_relogio` com a soma de `minutos_realizados` dos dias dentro da janela, chave `topo_x_coluna`. `app/folha/porta_export.py:515`, `app/folha/porta_export.py:699`. |
| App × tela | `AppLeATelaTest.test_MORDE_o_endpoint_nao_remonta_a_palavra_do_dia()` e `test_MORDE_o_endpoint_LE_o_espelho()` verificam a AST de `api_espelho_v2` para todos os recortes. `app/api/tests/test_r4_par6_app_le_a_tela.py:53`, `app/api/views.py:1361`. |
| Linhas × badge | `linha_x_badge(colab, 11, 2026, badge, soma=None)` usa o `resumo` do espelho e a soma das linhas. Chaves `linha_x_badge_divergentes`, `linha_x_badge_na_exportada` e `linha_x_badge_nao_medido`. `app/folha/porta_export.py:258`, `app/folha/porta_export.py:449`, `app/folha/porta_export.py:717`. |

`calendario_x_espelho` é uma chave de compatibilidade vazia, pois a pergunta foi incorporada a `espelho_x_dia_pago`. O selo CLI chama `vigiar_abertos`, `assumir` e `concluir`. `concluir` chama `lavrar`, que persiste `MetricaSnapshot` em `update_or_create`. A sonda usa apenas `medir` e os leitores. `app/folha/porta_export.py:608`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:76`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:197`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:219`, `app/folha/porta_export.py:785`, `app/folha/porta_export.py:1279`.

**B.** R5 so com escrita: fica fora (proibido medida nova). O script está fixado na comp 10. Executa duas passadas de `processar_cartorio(apply=True)` e `recalcular_fechamento_mes()`, com critério `foto3 == foto2`. O cartório carimba `CelulaDia` e pode emitir chamados. O recálculo grava `FechamentoMensal`. O dry do cartório omite `--apply` e lê a fila histórica `data < hoje`. O recálculo dispõe de `somente_leitura=True, lavrar_dias_pagos=False`. Os dois modos de leitura preservam o estado da primeira passagem e, por isso, não verificam o critério do R5. `bin/r5_idempotencia_frota.sh:24`, `bin/r5_idempotencia_frota.py:26`, `bin/r5_idempotencia_frota.py:104`, `bin/r5_idempotencia_frota.py:124`, `app/ponto/management/commands/processar_cartorio.py:25`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/services/cartorio.py:1122`, `app/ponto/services/cartorio.py:1883`, `app/ponto/services/cartorio.py:1905`, `app/ponto/services/fechamento.py:468`, `app/ponto/services/fechamento.py:1168`, `app/ponto/services/fechamento.py:1215`.

**F.** O wrapper chama um executável por `CODEX_SONDA_ENVELOPE`, com o caminho da sonda como único argumento. O executável existente `/home/ronald/saas-hasner/logs/FILA-CODEX-1811/sondas/envelope_sombra.sh:3` usa `exec` com `--sombra` e preserva o RC. `SONDA_ARVORE=<cópia conferida>/app` fixa os blobs montados. O Python deve capturar somente a exceção sentinela de rollback, fora do `atomic()`, pois RC diferente de zero faz o wrapper rejeitar a saída. O envelope faz escritas administrativas de role/GRANT e de arquivos durante a preparação. Eu apenas li o código. `bin/codex_pergunta.sh:627`, `bin/sonda_leitura.sh:66`, `bin/sonda_leitura.sh:126`, `bin/sonda_leitura.sh:140`, `bin/sonda_leitura.sh:157`, `bin/sonda_leitura.sh:232`, `bin/sonda_leitura.sh:239`.

## 27/11 15:09 · sessao 3656640e4966 · - · - · origem=Ronald (tmux)

- **Becos de disputa.** O contador usa `chamados.services.becos.classificar(disputa)` e retorna `FECHAVEL`, `BARRADA` ou `None`. O comando chama `get_user_sistema()` mesmo em dry ou `--alarme`, e esse helper usa `get_or_create`. A sonda deve usar o classificador diretamente. O `--alarme` do cron sai com RC 2 para qualquer `FECHAVEL`. `--apply` chama a porta de escrita `fechar_beco`. [app/chamados/services/becos.py:25](app/chamados/services/becos.py#L25), [app/chamados/management/commands/fechar_becos_disputa.py:27](app/chamados/management/commands/fechar_becos_disputa.py#L27), [app/core/utils.py:18](app/core/utils.py#L18), [app/config/crons.py:663](app/config/crons.py#L663)
- **Órfãs da lâmpada.** `chamados.reconciliador.orfas_pela_lampada_sem_evento(agora=REF, colaborador_ids=ids)` devolve os IDs completos. O recorte da competência pode filtrar esses IDs por `PerguntaDisputa.data_evento`. O comando sem argumentos de alarme conta em dry e `--alarme` sai com RC 2 se houver achados. `perguntas_orfas_reatavies(colab)` mede separadamente o universo elegível e pode receber o filtro de data no queryset. [app/chamados/reconciliador.py:1325](app/chamados/reconciliador.py#L1325), [app/ponto/management/commands/reconciliar_perguntas_orfas.py:24](app/ponto/management/commands/reconciliar_perguntas_orfas.py#L24), [app/chamados/reconciliador.py:1640](app/chamados/reconciliador.py#L1640)
- **Fio sem caminho.** `censo_alcance(empresas=[empresa], hoje=HOJE, agora=REF, limite=0)` usa a tarefa publicada e retorna todos os `sem_porta` mesmo com limite zero, com competência, causa e detalhe em cada linha. A sonda pode contar os `sem_porta` da `11/2026` por causa e detalhe. O vigia mede empresas visíveis em operação e competências que `competencias_a_lavrar` seleciona. O comando de frota grava carimbo em disco. `contador_fio_mudo(hoje=HOJE, agora=REF, ini=INI, fim=FIM, empresas=[empresa.pk])` mede os motivos devidos sem pergunta viva. `motivos_que_o_evento_deixou(cel, HOJE, REF, memo=memo)` mede o que o julgamento da célula deixou de emitir. [app/chamados/services/vigia_fio.py:524](app/chamados/services/vigia_fio.py#L524), [app/chamados/services/vigia_fio.py:622](app/chamados/services/vigia_fio.py#L622), [app/chamados/management/commands/vigiar_fio_morto.py:131](app/chamados/management/commands/vigiar_fio_morto.py#L131), [app/chamados/management/commands/disparar_perguntas_competencia.py:77](app/chamados/management/commands/disparar_perguntas_competencia.py#L77), [app/chamados/services/fabrica_da_celula.py:234](app/chamados/services/fabrica_da_celula.py#L234)
- **E6.** O comando `e6_oraculo --mes 11 --ano 2026 --hoje 2026-11-27`, sem CSV, certifica o espelho da frota e exige `FechamentoMensal` para selecionar colaboradores. O comando captura falha do espelho e segue. Para apor a classe a um dia já achado pelo vigia, existe precedente em `RELATO`, com `InsumosDoDono(colab, INI, FIM, HOJE, agora=REF).dono(dia)` como leitor por colaborador. [app/ponto/management/commands/e6_oraculo.py:128](app/ponto/management/commands/e6_oraculo.py#L128), [app/ponto/management/commands/e6_oraculo.py:182](app/ponto/management/commands/e6_oraculo.py#L182), [app/ponto/management/commands/e6_oraculo.py:276](app/ponto/management/commands/e6_oraculo.py#L276), [app/docs/RELATO.md:5549](app/docs/RELATO.md#L5549)
- **RC de órfãs de obra.** `bin/rc_sem_colher.py::sem_colher(raiz, relato_txt, agora=None, min_idade=0)` lê logs de obras das últimas 24 h. RC 0 indica lista vazia, RC 3 indica achados e RC 1 indica falha de leitura. Sua unidade é obra e arquivo `.rc`. [bin/rc_sem_colher.py:2](bin/rc_sem_colher.py#L2), [bin/rc_sem_colher.py:50](bin/rc_sem_colher.py#L50)

O envelope padrão de `codex_pergunta.sh --sonda` chama `sonda_leitura.sh` sem `--sombra`. Há um `CODEX_SONDA_ENVELOPE` de sombra documentado no `RELATO`. A saída só segue se o envelope retorna zero, logo a sonda precisa suprimir exclusivamente a exceção deliberada de rollback **fora** do `atomic`. `SET TRANSACTION READ ONLY` antes de consultar e a pré-carga `tranca_em_lote(sistema_pk=User.objects.values_list('pk', flat=True).get(username='sistema'))` evitam o `get_or_create` nas leituras da tranca. [bin/codex_pergunta.sh:623](bin/codex_pergunta.sh#L623), [bin/sonda_leitura.sh:93](bin/sonda_leitura.sh#L93), [app/docs/RELATO.md:2735](app/docs/RELATO.md#L2735), [app/chamados/juizes.py:1254](app/chamados/juizes.py#L1254), [app/chamados/juizes.py:1321](app/chamados/juizes.py#L1321)

## 27/11 15:08 · sessao 83229a591dba · - · - · origem=Ronald (tmux)

Auditei as 25 obras do bloco inicial e as cinco verificações adicionais. Enviei ao agente principal as classificações, os sítios de código, os contratos e as ressalvas de acervo. A confirmação do hash e a situação atual em produção ficam [nao verificado].

## 27/11 15:08 · sessao 9e7bed9a78ac · INVENTARIO-PELA-REGRA · leitura · origem=Code

# INVENTÁRIO-PELA-REGRA

Gabarito de **12 casos CCT e 5 casos intermitentes**, pela L-110. Execução contra o HEAD e placar de vermelhos [nao verificado]. `app/docs/LEIS.md:136`

Os cadastros, vínculos, convocações e batidas abaixo são dados de teste [nao verificado]. As batidas estão em UTC−3. Nos plantões que cruzam a meia-noite, os minutos pertencem ao dia de início. `app/docs/LEIS.md:107` · `app/docs/LEIS.md:117`

A extração inicial exibiu comentários técnicos junto dos jurídicos em `app/core/regua_cct.py:56`. As referências dos casos apontam para as regras, cláusulas e pareceres usados no gabarito. O corte de 17/11 sobre intermitentes e seu texto em `logs/AVAIS-17*` ficaram [nao verificado]. Os casos INT usam a CLT e as leis da casa citadas em cada linha.

## CCT dos Vigilantes de Londrina

`Vigilância A` é a empresa fictícia. Em todos os casos CCT, a linha de `AplicacaoConvencao` está ativa e é única no nível empresa + praça, para o Sindicato nº 2. O vínculo 12x36 está vigente desde 01/11/2026. A convenção PR000251/2026 tem vigência documentada de 01/02/2026 a 31/01/2028. `CLAUDE.md:214` · `CLAUDE.md:218` · `app/core/regua_cct.py:234`

| ID | Família | Escala | CADASTRO | BATIDAS (UTC−3) | RESPOSTA DA REGRA | FONTE |
|---|---|---|---|---|---|---|
| CCT-01 | CCT · `regua_excedente` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026 | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | `lei: cláusula ou corte que defina se a régua do escrito e a 9ª diária se aplicam ao plantão regular 12x36 e quais minutos entram como HE nesse ciclo.` | `SERIE_T.md:1072` · parecer e decisão de 29–30/07, `app/core/regua_cct.py:376` e `app/core/regua_cct.py:378` |
| CCT-02 | CCT · `prorrogacao_noturna_pos5h` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Leitura restritiva registrada no cadastro | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | `lei: parecer ou corte que confirme se a cl. 10 exclui os minutos trabalhados entre 05h e 07h. O texto referido fecha a janela 22h–05h e é silente sobre a prorrogação.` | L-035, `app/docs/LEIS.md:83` · cl. 10, `CLAUDE.md:236` · registro da interpretação, `SERIE_T.md:1326` |
| CCT-03 | CCT · `hora_reduzida_afastada_12x36` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Janela 22h–05h fixada no caso | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | Normais **660 min**. Noturno 0025 **420 min de base**, com conversão **1:1** entre relógio e folha, a 20%. | Cl. 38-d, `app/core/regua_cct.py:129` e `app/core/regua_cct.py:136` · L-036, `app/docs/LEIS.md:84` · `SERIE_T.md:1074` |
| CCT-04 | CCT · `adicional_noturno_pct` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Janela 22h–05h fixada no caso | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | Normais **660 min**. Rubrica 0025 **420 min de base a 20%**, com adicional devido além do piso salarial. | Cl. 3 e 38-d, `app/core/regua_cct.py:136` e `app/core/regua_cct.py:146` · `SERIE_T.md:1075` |
| CCT-05 | CCT · `tolerancia_marcacao_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa prevista 20h30–21h30 | 18/11 E 19:06, S 20:30, E 21:30. 19/11 S 07:00 | `lei: cláusula que fixe a tolerância CCT por marcação, ou corte que mande usar o piso legal neste cadastro. A entrada isolada tem desvio de 6 min.` | `SERIE_T.md:1076` · corte sobre o piso da CLT, art. 58 § 1º, `app/docs/CORTES.md:521` |
| CCT-06 | CCT · `tolerancia_dia_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa prevista 20h30–21h30 | 18/11 E 19:04, S 20:30, E 21:34. 19/11 S 06:56 | `lei: cláusula que fixe o teto diário CCT, ou corte que mande usar o piso legal neste cadastro. Os três desvios de 4 min somam 12 min no mesmo plantão.` | `SERIE_T.md:1077` · corte sobre o piso da CLT, art. 58 § 1º, `app/docs/CORTES.md:521` · L-085, `app/docs/LEIS.md:117` |
| CCT-07 | CCT · `intrajornada_minima_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa prevista 20h30–21h30 e `intervalo_indenizavel=True` | 18/11 E 19:00, S 20:30, E 21:00. 19/11 S 07:00 | `lei: cláusula que fixe o mínimo convencional aplicável. Com 30 min de pausa gozada, esse mínimo decide quantos minutos suprimidos entram na rubrica de intrajornada indenizada.` | `SERIE_T.md:1078` · L-031, `app/docs/LEIS.md:79` · [CLT, art. 71, del5452compilado.htm:373 e :379](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art71) |
| CCT-08 | CCT · `compensacao_autorizada` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantões 18 e 26/11/2026. Prorrogação autorizada e compensação solicitada no segundo plantão | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:30. 26/11 E 19:00, S 20:30, E 21:30. 27/11 S 06:30 | `lei: cláusula que autorize compensar a prorrogação no 12x36 e defina se os 30 min podem ser quitados pela redução do segundo plantão.` | `SERIE_T.md:1079` · [CLT, art. 59 §§ 2º, 5º e 6º, del5452compilado.htm:326, :330 e :332](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art59) |
| CCT-09 | CCT · `compensacao_janela` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantões 18/11/2026 e 18/01/2027. Compensação admitida como premissa para testar só o prazo | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:30. 18/01 E 19:00, S 20:30, E 21:30. 19/01 S 06:30 | `lei: cláusula que fixe a janela, o marco inicial e o vencimento do saldo. Isso decide se a redução em janeiro quita os 30 min de novembro ou se o saldo já virou HE a pagar.` | `SERIE_T.md:1080` · [CLT, art. 59 §§ 2º, 5º e 6º, del5452compilado.htm:326, :330 e :332](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art59) |
| CCT-10 | CCT · `teto_prorrogacao_dia_min` | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Prorrogação de 90 min determinada e confirmada pelo empregador | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 08:30 | `lei: cláusula que fixe o teto diário convencional no 12x36 e diga em que rubrica pagar o trabalho efetivo acima do teto, se houver.` | `SERIE_T.md:1081` · L-097, `app/docs/LEIS.md:115` · [CLT, arts. 59 e 59-A, del5452compilado.htm:323 e :333](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art59) |
| CCT-11 | CCT · cl. 10, janela estrita | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Sem prorrogação após 05h por premissa expressa do caso | 18/11 E 19:00, S 20:30, E 21:30. 19/11 S 07:00 | Normais **660 min**. Noturno 0025 **420 min de base a 20%**, sendo 120 min em 18/11 e 300 min em 19/11. De 05h a 07h, **0 min de adicional noturno**. O plantão é atribuído a 18/11. | Cl. 10, `CLAUDE.md:236` · cl. 38-d, `app/core/regua_cct.py:136` · L-085, `app/docs/LEIS.md:117` |
| CCT-12 | CCT · cl. 38-d, hora de 60 min | 12x36 noturno, 19h–07h | Vigilância A / `cct` / PR000251/2026 / Londrina / aplicação ativa empresa + praça / vínculo 12x36 / plantão 18/11/2026. Pausa 01h–02h e janela 22h–05h fixada no caso | 18/11 E 19:00. 19/11 S 01:00, E 02:00, S 07:00 | Normais **660 min**. Noturno 0025 **360 min de base a 20%**. Dos 420 min da janela, 60 min são pausa. Cada 60 min de trabalho noturno gera 60 min de base. | Cl. 38-d e parecer de 01/09, `app/core/regua_cct.py:129`, `app/core/regua_cct.py:136` e `app/core/regua_cct.py:141` · L-036, `app/docs/LEIS.md:84` |

## Intermitente

`Serviços B` é a empresa fictícia. Os casos usam regime `clt`, praça Londrina, sem CCT ou `AplicacaoConvencao` aplicável, vínculo intermitente escrito desde 01/11/2026 e salário-hora pactuado conforme o piso aplicável. As convocações indicadas foram recebidas e aceitas na data registrada, às 08h. [CLT, art. 452-A, del5452compilado.htm:2116, :2117 e :2119](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a)

| ID | Família | Escala | CADASTRO | BATIDAS (UTC−3) | RESPOSTA DA REGRA | FONTE |
|---|---|---|---|---|---|---|
| INT-01 | Intermitente · convocação aceita e trabalhada | Personalizado sem intervalo, 08h–12h | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / chamada aceita 19/11 para 23/11/2026, 08h–12h | 23/11 E 08:00, S 12:00 | Normais **240 min**, HE **0 min**, falta **0 min**. Ao fim da prestação, recibo com remuneração, férias proporcionais + 1/3, 13º proporcional e DSR discriminados. | [CLT, arts. 71 § 1º e 452-A §§ 1º, 2º, 6º e 7º, del5452compilado.htm:375, :2117, :2119, :2125 e :2135](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) |
| INT-02 | Intermitente · dia sem convocação | Personalizado sem intervalo, sem jornada chamada no dia | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / 24/11/2026 sem convocação | 24/11 sem batidas | Dia de inatividade. Previsto **0 min**, normais **0 min**, HE **0 min**, falta **0 min**. Sem desconto de DSR por falta nesse dia. | L-088, `app/docs/LEIS.md:120` · [CLT, art. 452-A § 5º, del5452compilado.htm:2123](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) |
| INT-03 | Intermitente · convocação em feriado | Personalizado sem intervalo, 09h–13h | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / chamada avulsa aceita 17/11 para o feriado de teste de 20/11/2026, sem plantão regular na escala e sem folga compensatória | 20/11 E 09:00, S 13:00 | Feriado em dobro **240 min**, pagos a **2×** o valor da hora normal. DSR em parcela própria. | [Lei 605/49, art. 9º, l0605.htm:67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm#art9) · [Súmula 146, TST, IRR-308, p. 4](https://www.tst.jus.br/documents/d/guest/irr-308-_acordao-pdf) · L-086, `app/docs/LEIS.md:118` |
| INT-04 | Intermitente · HE acima do convocado | Personalizado sem intervalo, 08h–11h | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / chamada aceita 19/11 para 25/11/2026, 08h–11h. HE até 12h acordada e autorizada com registro | 25/11 E 08:00, S 12:00 | Normais **180 min**, HE50 **60 min**, falta **0 min**. | L-097, `app/docs/LEIS.md:115` · [CLT, arts. 59 § 1º e 452-A § 6º I e V, del5452compilado.htm:325, :2127 e :2134](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) |
| INT-05 | Intermitente · DSR proporcional | Personalizado sem intervalo, 08h–11h em dois dias | Serviços B / `clt` / sem CCT / Londrina / sem aplicação / vínculo intermitente / uma chamada aceita 19/11 para 23 e 24/11/2026. Ambos os dias cumpridos integralmente | 23/11 E 08:00, S 11:00. 24/11 E 08:00, S 11:00 | `lei: fórmula do DSR proporcional para uma convocação de dois dias, com base, divisor e arredondamento em minutos. O DSR é devido ao fim da prestação e vem discriminado no recibo.` | [CLT, art. 452-A §§ 6º IV e 7º, del5452compilado.htm:2125, :2132 e :2135](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) · [Lei 605/49, art. 7º b, l0605.htm:55](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm#art7) |

## Perguntas de lei

1. **CCT-01.** Qual cláusula ou corte define a incidência da régua do escrito e da 9ª diária sobre o 12x36 regular? `app/core/regua_cct.py:378`
2. **CCT-02.** Qual parecer ou corte confirma o tratamento de 05h–07h pela cl. 10? `CLAUDE.md:236` · `SERIE_T.md:1326`
3. **CCT-05 e CCT-06.** Quais são as tolerâncias convencionais por marcação e por plantão? Na ausência de cláusula própria, vale neste cadastro o par legal de 5 min por marcação e 10 min por dia? `SERIE_T.md:1076` · `SERIE_T.md:1077` · `app/docs/CORTES.md:521`
4. **CCT-07.** Qual mínimo de intrajornada da CCT rege a indenização do intervalo parcialmente gozado no 12x36? `SERIE_T.md:1078` · `app/docs/LEIS.md:79`
5. **CCT-08 e CCT-09.** Qual cláusula autoriza compensar prorrogação no 12x36 e qual é o prazo para quitar o saldo? `SERIE_T.md:1079` · `SERIE_T.md:1080`
6. **CCT-10.** Qual teto convencional rege a prorrogação diária no 12x36 e como se classifica, para pagamento, o trabalho efetivo acima dele? `SERIE_T.md:1081` · `app/docs/LEIS.md:115`
7. **INT-05.** Qual é a base, o divisor e o arredondamento do DSR proporcional em minutos por período de prestação? [CLT, art. 452-A § 6º IV, del5452compilado.htm:2132](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art452a) · [Lei 605/49, art. 7º b, l0605.htm:55](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm#art7)
8. **INT-01 a INT-05.** Qual é o texto do corte de 17/11 sobre intermitentes e quais destes casos ele alcança? [nao verificado]

## 27/11 15:08 · sessao b24bb17059ed · - · - · origem=Ronald (tmux)

Auditei as 54 obras entre `FERIADO-ESCALA` e `O-JANELA-HE` e encaminhei as linhas ao agente principal, com sítios de código, estado no recorte e contrato aplicável. [GRADE-MESTRA.md](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/GRADE-MESTRA.md:140)

No agregado, `GRADE-FOLGA-C5` e `GRADE-FOLGA-GERADOR` compartilham a leitura de folga fora do juiz. `PROPOSITOR-JANELA-LONGA` e `FILA-POR-CAUSA-AS-OF` compartilham o uso da última lavra onde a consulta precisa da decisão por dia. `TROCA-DE-POSTO-SEM-VINCULO` e o item 01 de `BO2910-ESTRUTURA` compartilham a troca de posto sem uma porta única para o ato. `app/ponto/precedencia.py:212`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1561`, `app/escala/models.py:1321`, `app/escala/services/propositor.py:493`, `app/chamados/services/fila_por_causa.py:184`, `app/colaboradores/views.py:337` e `app/colaboradores/views.py:1415`.

## 27/11 15:08 · sessao 903d81c0f68d · INVENTARIO-PELA-REGRA · leitura · origem=Code

# INVENTARIO-PELA-REGRA

As competências usadas são **11/2026** de 21/10 a 20/11, **12/2026** de 21/11 a 20/12, **01/2027** de 21/12 a 20/01 e **02/2027** de 21/01 a 20/02. Os 15 casos são gabaritos de leitura, com asserções limitadas aos dias informados. A execução na pista e o HEAD estão [nao verificado]. [app/docs/LEIS.md:77](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:77) · [app/docs/LEIS.md:136](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:136)

**Dados comuns.** Cadastro e batidas são entradas sintéticas, independentes por caso. Onde constar 6×1 comercial, usei 08:00–17:00 com pausa 12:00–13:00 de segunda a sexta, sábado 08:00–12:00 e DSR domingo. Usei essa escala nos recortes anteriores à transição de dezembro da EC 141/2026. Nos plantões de dezembro e janeiro, o acordo coletivo de jornada e repouso regular é dado do teste. Os feriados locais são entradas de cadastro da praça Londrina/PR, cuja situação real está [nao verificado]. [app/docs/LEIS.md:64](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:64) · [EC 141/2026, arts. 6º e 10, linhas 34–52](https://www.planalto.gov.br/ccivil_03/constituicao/emendas/emc/emc141.htm)

**CMP-01**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo, mensalista. Feriado nacional de 20/11/2026 cadastrado. Sexta seria dia de trabalho e o posto dispensa o expediente no feriado.  
**BATIDAS (UTC−3)** 19/11 08:00 E, 12:00 S, 13:00 E, 17:00 S. 20/11 sem batidas. 21/11 08:00 E, 12:00 S.  
**RESPOSTA DA REGRA** 20/11 é feriado nacional e fecha a comp. **11/2026**. No dia, 0 min de presença exigida, 0 trabalhados, 0 de HE de feriado e 0 de falta. Remuneração do feriado preservada. O sábado 21/11 tem 240 min normais na comp. **12/2026**.  
**FONTE** `app/docs/LEIS.md:77` · [Lei 14.759/2023, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2023/lei/l14759.htm) · [Lei 605/1949, art. 8º, linha 64](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-02**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo. Cadastro dado de feriado municipal em 10/12/2026, praça Londrina/PR. Trabalho autorizado em dia ordinariamente útil, sem folga compensatória. DSR em 13/12.  
**BATIDAS (UTC−3)** 10/12 08:00 E, 12:00 S, 13:00 E, 17:00 S. 11/12 08:00 E, 12:00 S, 13:00 E, 17:00 S. 12/12 08:00 E, 12:00 S. 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. Em 10/12, 480 min trabalhados na base da rubrica **feriado em dobro**, remunerada a 200%. Em 11/12, 480 min normais. Em 12/12, 240 min normais. DSR de 13/12 preservado.  
**FONTE** `app/docs/LEIS.md:64` · `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · [Lei 605/1949, art. 9º, linha 67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [Súmula 146 do TST, linhas 1498–1502](https://www.tst.jus.br/documents/10157/cc23ec94-9e08-47e3-8ed6-2e662cee7690)

**CMP-03**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo. Mesmo feriado municipal dado de 10/12/2026. Trabalho autorizado, com folga compensatória integral formalizada e gozada em 11/12. DSR em 13/12.  
**BATIDAS (UTC−3)** 10/12 08:00 E, 12:00 S, 13:00 E, 17:00 S. 11/12 sem batidas. 12/12 08:00 E, 12:00 S. 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. Em 10/12, 480 min trabalhados e 0 min de dobra de feriado. Em 11/12, folga compensatória com 0 min trabalhados e 0 de falta. Em 12/12, 240 min normais. DSR de 13/12 preservado.  
**FONTE** `app/docs/LEIS.md:77` · [Lei 605/1949, arts. 6º e 9º, linhas 28 e 67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [Súmula 146 do TST, linhas 1498–1502](https://www.tst.jus.br/documents/10157/cc23ec94-9e08-47e3-8ed6-2e662cee7690)

**CMP-04**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026, vínculo ativo, mensalista. Cadastro dado de feriado estadual em 13/12/2026, aplicável à praça Londrina/PR. A data coincide com o DSR da escala. Semana cumprida.  
**BATIDAS (UTC−3)** 07, 08, 09, 10 e 11/12, cada dia 08:00 E, 12:00 S, 13:00 E, 17:00 S. 12/12 08:00 E, 12:00 S. 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. Em 13/12, 0 min previstos de presença, 0 trabalhados, 0 de HE de feriado e 0 de falta. Um DSR preservado no salário mensal, com 0 min de dobra pelo encontro das duas datas.  
**FONTE** `app/docs/LEIS.md:64` · `app/docs/LEIS.md:74` · `app/docs/LEIS.md:77` · [Lei 605/1949, arts. 1º e 6º–9º, linhas 15–67](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-05**  
**Família** escala  
**Escala** 12×36 diurno, 07:00–19:00  
**CADASTRO** Admissão 01/10/2026, vínculo ativo. Feriado nacional de 25/12/2026 cadastrado. Foto escala 25/12 como plantão. Acordo coletivo válido e sem cláusula de dobra. Pausa 12:00–13:00 e limites de jornada cumpridos.  
**BATIDAS (UTC−3)** 24/12 sem batidas. 25/12 07:00 E, 12:00 S, 13:00 E, 19:00 S. 26/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **01/2027**. Em 25/12, 660 min previstos e normais trabalhados, 0 de HE50, 0 de dobra de feriado, 0 de adicional noturno, 0 de intrajornada indenizada. DSR e feriado abrangidos pela remuneração mensal pactuada. A Resolução TST 225/2025 cancelou a Súmula 444 por perda de eficácia a partir de 11/11/2017.  
**FONTE** `app/docs/LEIS.md:73` · `app/docs/LEIS.md:77` · `app/docs/LEIS.md:118` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm) · [CLT, art. 59-A, linhas 333–334](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Resolução TST 225/2025, art. 1º, XXIV, p. 3](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/92286/Resolu%C3%A7%C3%A3o%20n.%20225%2C%20de%2030%20de%20junho%20de%202025.pdf?sequence=3#page=3)

**CMP-06**  
**Família** escala  
**Escala** 12×36 noturno, 19:00–07:00, CCT Vigilantes Londrina  
**CADASTRO** Admissão 01/10/2026, vínculo ativo e aplicação da CCT cadastrada. Feriado nacional de 01/01/2027 cadastrado. Foto escala plantão iniciado em 01/01. Pausa 21:00–22:00.  
**BATIDAS (UTC−3)** 31/12 sem batidas. 01/01 19:00 E, 21:00 S, 22:00 E. 02/01 07:00 S.  
**RESPOSTA DA REGRA** `lei: cláusula vigente que conceda dobra ao 12×36 dos vigilantes e defina sua base`. Na comp. **01/2027**, são certos 660 min trabalhados e 420 min na rubrica 0025 a 20%, com hora de 60 min. São 240 min trabalhados na data civil do feriado. Para atribuição da jornada, a L-085 usa 01/01, dia de início.  
**FONTE** `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:118` · `CLAUDE.md:227` · `CLAUDE.md:236` · `app/core/regua_cct.py:473` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm) · [CLT, art. 59-A, linhas 333–334](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm)

**CMP-07**  
**Família** escala  
**Escala** 12×36 noturno, 19:00–07:00, CCT Vigilantes Londrina  
**CADASTRO** Admissão 01/10/2026, vínculo ativo e aplicação da CCT cadastrada. Natal de 25/12/2026 cadastrado. Foto escala plantão iniciado em 24/12. Pausa 21:00–22:00.  
**BATIDAS (UTC−3)** 24/12 19:00 E, 21:00 S, 22:00 E. 25/12 07:00 S.  
**RESPOSTA DA REGRA** `lei: cláusula de dobra e corte de incidência por data civil que resolva a divergência com a L-085`. Na comp. **01/2027**, são certos 660 min trabalhados e 420 min na rubrica 0025 a 20%. O Natal toca fisicamente 25/12 das 00:00 às 07:00, 420 min, dos quais 300 dentro da janela noturna. A L-085 atribui o plantão a 24/12.  
**FONTE** `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:118` · `CLAUDE.md:236` · `app/core/regua_cct.py:473` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm)

**CMP-08**  
**Família** escala  
**Escala** 12×36 noturno, 19:00–07:00, CCT Vigilantes Londrina  
**CADASTRO** Admissão 01/10/2026, vínculo ativo e aplicação da CCT cadastrada. Feriado de 01/01/2027 cadastrado. Foto escala plantão iniciado em 31/12. Pausa 21:00–22:00.  
**BATIDAS (UTC−3)** 31/12 19:00 E, 21:00 S, 22:00 E. 01/01 07:00 S.  
**RESPOSTA DA REGRA** `lei: cláusula de dobra e corte de incidência por data civil que resolva a divergência com a L-085`. Na comp. **01/2027**, são certos 660 min trabalhados e 420 min na rubrica 0025 a 20%. O Ano-Novo toca fisicamente 01/01 das 00:00 às 07:00, 420 min, dos quais 300 dentro da janela noturna. A L-085 atribui o plantão a 31/12. A virada do ano civil permanece na mesma competência.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:118` · `CLAUDE.md:236` · `app/core/regua_cct.py:473` · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm)

**CMP-09**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão e início de vínculo em 05/12/2026. Horista. DSR em 06/12. Sem feriado cadastrado no recorte.  
**BATIDAS (UTC−3)** 04/12 sem batidas. 05/12 08:00 E, 12:00 S. 06/12 sem batidas.  
**RESPOSTA DA REGRA** `lei: fórmula e arredondamento do DSR proporcional da primeira semana do horista admitido no sábado`. Na comp. **12/2026**, antes de 05/12 são 0 min previstos, 0 de falta e 0 de DSR creditável. Em 05/12 são 240 min previstos e normais trabalhados. O primeiro DSR a apurar é 06/12, na mesma competência.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:157` · [Lei 605/1949, arts. 6º e 7º, linhas 28–61](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-10**  
**Família** escala  
**Escala** 6×1 comercial  
**CADASTRO** Admissão e início de vínculo em 21/11/2026. Horista. Feriado nacional de 20/11 cadastrado. DSR em 22/11.  
**BATIDAS (UTC−3)** 20/11 sem batidas. 21/11 08:00 E, 12:00 S. 22/11 sem batidas.  
**RESPOSTA DA REGRA** `lei: fórmula e arredondamento do DSR proporcional do horista admitido no primeiro dia da competência`. Em **11/2026**, 0 min previstos, trabalhados, de falta ou de DSR e nenhum crédito do feriado de 20/11 para este vínculo. Em **12/2026**, 240 min previstos e normais trabalhados em 21/11. O primeiro DSR a apurar é 22/11.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:157` · [Lei 14.759/2023, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2023/lei/l14759.htm) · [Lei 605/1949, arts. 6º–8º, linhas 28–64](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm)

**CMP-11**  
**Família** escala  
**Escala** personalizada sem intervalo, 08:00–12:00 de segunda a sexta  
**CADASTRO** Admissão e início de vínculo em 20/01/2027. Horista. Repousos remunerados em 23 e 24/01. Sem feriado cadastrado no recorte.  
**BATIDAS (UTC−3)** 19/01 sem batidas. 20, 21 e 22/01, cada dia 08:00 E, 12:00 S. 23 e 24/01 sem batidas.  
**RESPOSTA DA REGRA** `lei: fórmula e arredondamento dos dois DSR proporcionais quando a primeira semana atravessa o corte`. São 0 min previstos e de falta antes da admissão, 240 min previstos e normais em **01/2027** no dia 20, e 480 min previstos e normais em **02/2027** nos dias 21 e 22. Os dois repousos a apurar caem em **02/2027**.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:157` · [CLT, art. 71, § 1º](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Lei 605/1949, arts. 6º–7º, linhas 28–61](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [EC 141/2026, arts. 1º e 10, linhas 9–26 e 49–52](https://www.planalto.gov.br/ccivil_03/constituicao/emendas/emc/emc141.htm)

**CMP-12**  
**Família** folha/export  
**Escala** personalizada sem intervalo, 08:00–12:00 de segunda a sexta  
**CADASTRO** Admissão 01/10/2026. Desligamento efetivado em 20/01/2027, último dia do vínculo. Sem feriado no recorte. Há pendência de célula que normalmente reteria o TXT. Os demais requisitos da emissão estão satisfeitos.  
**BATIDAS (UTC−3)** 18, 19 e 20/01, cada dia 08:00 E, 12:00 S. 21/01 sem batidas.  
**RESPOSTA DA REGRA** **01/2027** inclui o dia 20. São 720 min previstos e normais no recorte de 18 a 20, dos quais 240 no último dia. O movimento entra no TXT de **01/2027** e a pendência do desligado gera zero motivos de retenção. Em 21/01, já em **02/2027**, são 0 min previstos, 0 de falta e nenhum movimento para este vínculo.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · `app/docs/CORTES.md:517`. A inclusão no TXT é asserção do aval informado. Seu literal nos documentos consultados está [nao verificado].

**CMP-13**  
**Família** folha/export  
**Escala** personalizada sem intervalo, 08:00–12:00 de segunda a sexta  
**CADASTRO** Admissão 01/10/2026. Desligamento efetivado em 21/12/2026, último dia do vínculo. Descansos em 19 e 20/12. Sem feriado no recorte. Demais requisitos de emissão satisfeitos.  
**BATIDAS (UTC−3)** 18/12 08:00 E, 12:00 S. 19 e 20/12 sem batidas. 21/12 08:00 E, 12:00 S. 22/12 sem batidas.  
**RESPOSTA DA REGRA** 240 min normais de 18/12 no TXT de **12/2026** e 240 min normais de 21/12 no TXT de **01/2027**. Zero motivos de retenção do desligado em ambas. A partir de 22/12, 0 min previstos e 0 de falta.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · `app/docs/CORTES.md:517`. A inclusão no TXT é asserção do aval informado. Seu literal nos documentos consultados está [nao verificado].

**CMP-14**  
**Família** folha/export  
**Escala** 6×1 comercial  
**CADASTRO** Admissão 01/10/2026. Desligamento efetivado em 10/12/2026, último dia do vínculo. Sem feriado cadastrado neste recorte. Demais requisitos de emissão satisfeitos.  
**BATIDAS (UTC−3)** 07, 08, 09 e 10/12, cada dia 08:00 E, 12:00 S, 13:00 E, 17:00 S. 11, 12 e 13/12 sem batidas.  
**RESPOSTA DA REGRA** Comp. **12/2026**. São 1.920 min previstos e normais no recorte de 07 a 10/12, dos quais 480 no último dia. O movimento entra no TXT, com zero motivos de retenção do desligado. Em 11, 12 e 13/12, 0 min previstos, 0 de falta e nenhum DSR por data posterior ao fim do vínculo.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · `app/docs/CORTES.md:517` · `app/docs/DOSSIES.md:135`. A inclusão no TXT é asserção do aval informado. Seu literal nos documentos consultados está [nao verificado].

**CMP-15**  
**Família** ausência/férias  
**Escala** 6×1 comercial nos dias do recorte  
**CADASTRO** Admissão 01/10/2025, vínculo ativo, mensalista. Férias já concedidas de 15/11 a 14/12/2026, inclusive. Feriados nacionais de 15 e 20/11 cadastrados.  
**BATIDAS (UTC−3)** Cada dia de 15/11 a 14/12, sem batidas. 15/12 08:00 E, 12:00 S, 13:00 E, 17:00 S.  
**RESPOSTA DA REGRA** **11/2026** recebe 6 dias corridos de gozo, de 15 a 20/11. **12/2026** recebe 24, de 21/11 a 14/12. O dia 21/11 está em férias. Os DSR de 15, 22 e 29/11 e de 06 e 13/12 estão dentro do gozo. Durante o gozo são 0 min de trabalho, falta, HE ou DSR pago à parte. O retorno em 15/12 dá 480 min normais em **12/2026**. O terço mínimo incide sobre a remuneração dos 30 dias e é pago com ela até 13/11, data que cai em **11/2026**. `lei: competência e formato da rubrica de férias e terço no TXT`.  
**FONTE** `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · [CLT, arts. 130, 142 e 145](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Constituição, art. 7º, XVII, linha 295](https://www.planalto.gov.br/ccivil_03/constituicao/constituicao.htm) · [Lei 662/1949, art. 1º, linha 10](https://www.planalto.gov.br/ccivil_03/leis/l0662.htm)

## Perguntas de lei

1. **Dobra do 12×36, CMP-05 a CMP-08.** Qual cláusula ou corte vigente concede dobra ao plantão previsto? Para o cadastro sem cláusula favorável, o aval confirma o piso do art. 59-A? `app/docs/LEIS.md:118` · `app/core/regua_cct.py:473` · [CLT, art. 59-A, linhas 333–334](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm) · [Resolução TST 225/2025, art. 1º, XXIV, p. 3](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/92286/Resolu%C3%A7%C3%A3o%20n.%20225%2C%20de%2030%20de%20junho%20de%202025.pdf?sequence=3#page=3)
2. **Virada do feriado, CMP-06 a CMP-08.** Havendo dobra, qual corte autoriza calcular a rubrica pelas horas da data civil, inclusive a sobreposição com a 0025, diante da atribuição pelo início na L-085? `app/docs/LEIS.md:84` · `app/docs/LEIS.md:117` · `CLAUDE.md:236`
3. **DSR da primeira semana, CMP-09 a CMP-11.** Qual base, divisor e arredondamento convertem o DSR proporcional do horista em minutos? Como aplicar a fórmula aos dois repousos quando a semana cruza o dia 20/21? `app/docs/LEIS.md:77` · [Lei 605/1949, arts. 6º–7º, linhas 28–61](https://www.planalto.gov.br/ccivil_03/leis/l0605.htm) · [EC 141/2026, arts. 1º e 10, linhas 9–26 e 49–52](https://www.planalto.gov.br/ccivil_03/constituicao/emendas/emc/emc141.htm)
4. **Férias e terço no TXT, CMP-15.** Qual regra atribui o pagamento no TXT quando o gozo atravessa o corte? Qual unidade a rubrica deve carregar? `app/docs/LEIS.md:77` · `app/docs/LEIS.md:113` · [CLT, arts. 142 e 145](https://www.planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm)

## 27/11 15:08 · sessao 9139767f560b · - · - · origem=Ronald (tmux)

Auditei as 50 obras de `PERGUNTA-VIVA-COM-CELULA-ABSOLVEU` a `F-346-FILA-OPUS` e enviei a tabela em três blocos para consolidação. Os endereços do anexo divergem da cópia em várias obras. Por exemplo, a primeira consta em `app/docs/BACKLOG.md:2451` e a última em `app/docs/BACKLOG.md:2508`. Estado de produção e confirmação do hash Git ficam como [nao verificado].

## 27/11 15:08 · sessao 39cd61c86174 · - · - · origem=Ronald (tmux)

**D.** A view chama `recalcular_fechamento_mes(11, 2026, empresa_id=e.pk, somente_leitura=True, retornar_objeto=True)`, usa os valores do dicionário como `fechamentos`, chama `classificar_export(e, 11, 2026, fechamentos=fechamentos)` e passa os mesmos objetos com `itens=itens` a `montar_linhas_export`. Em seguida chama `validar_layout_dominio(linhas)`. A montagem devolve `(linhas, entra_ids)` em memória. A aprovação e o registro do TXT ficam na view. [app/folha/views.py:214](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:214) [app/folha/views.py:258](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:258) [app/folha/views.py:275](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:275) [app/folha/views.py:282](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:282) [app/folha/views.py:304](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:304) [app/folha/export.py:699](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:699)

Para atribuir os erros, monte novamente com `[fechamento]` e `[item]` na ordem original e use `fechamento.colaborador_id`. Preserve cada bloco inteiro, pois o validador conta os registros 11 depois da rubrica 8792 conjuntamente. Confronte a concatenação das linhas e dos erros com a rodada da empresa. Publique somente a classificação da mensagem e seu campo, pois algumas mensagens embutem o registro. [app/folha/export.py:266](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:266) [app/folha/export.py:725](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:725) [app/folha/export.py:734](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:734)

Fixe o relógio da medição com `freeze_time` e capture a hora real do cabeçalho antes do congelamento. O fechamento lê o relógio internamente e captura exceções por colaborador. Compare as chaves retornadas com `universo_da_competencia` e falhe com os IDs ausentes, ou propague a exceção original no ponto de log. [app/requirements.txt:39](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/requirements.txt:39) [app/ponto/services/fechamento.py:574](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:574) [app/ponto/services/fechamento.py:636](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:636) [app/ponto/services/fechamento.py:1327](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1327)

**E.** O cadastro é `core.models.Feriado`, tabela `core_feriado`. Use `Feriado.objects.filter(abrangencia='municipal', data__range=(date(2026, 11, 21), date(2026, 12, 20))).order_by('uf', 'municipio', 'data', 'pk')`. Para cruzar por empresa e praça, leia os pares `Posto.praca` e `Posto.municipio` da empresa e use `core.feriados.alcanca(registro, posto.municipio)`, que normaliza UF e nome do município. [app/core/models.py:5](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/models.py:5) [bin/sombra_regras_pessoais.py:194](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sombra_regras_pessoais.py:194) [app/colaboradores/models.py:219](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/colaboradores/models.py:219) [app/core/feriados.py:57](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:57)

O cenário de teste identifica **10/12/2026, Aniversário de Londrina/PR**. A presença do registro na Sombra permanece [nao verificado]. [app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:8](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:8) [app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:62](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:62)

**Envelope.** O adaptador existente encaminha a `--sombra`. Configure-o via `CODEX_SONDA_ENVELOPE` e a árvore fixada via `SONDA_ARVORE`. Capture exclusivamente a exceção sentinela após o `atomic` reverter, pois o wrapper descarta toda a saída quando o envelope termina com código diferente de zero. [Adaptador da Sombra](/home/ronald/saas-hasner/logs/FILA-CODEX-1811/sondas/envelope_sombra.sh:3) [bin/sonda_leitura.sh:226](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/sonda_leitura.sh:226) [bin/codex_pergunta.sh:628](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/bin/codex_pergunta.sh:628)

## 27/11 15:07 · sessao d1951f170791 · - · - · origem=Ronald (tmux)

Auditei os 40 IDs atribuídos e encaminhei as linhas e os cruzamentos com o placar. Os sítios, a classificação e as ressalvas de cada obra foram enviados nos três blocos. A confirmação do hash da cópia e a incidência atual em produção permanecem [nao verificado].

## 27/11 15:07 · sessao db709b7df738 · - · - · origem=Ronald (tmux)

Auditei os 48 IDs atribuídos e enviei as linhas ao agente principal em cinco blocos. A atualização final inclui o exemplo vivo de carimbo desatualizado em `app/ponto/models.py:180` e `app/ponto/services/justificativa.py:351–353`. A prevalência em produção ficou `[nao verificado]`.

## 27/11 15:06 · sessao 313d857a7dc2 · INVENTARIO-PELA-REGRA · leitura · origem=Code

## INVENTARIO-PELA-REGRA — BATIDA/ESPURIA

15 casos escritos pela regra, com cadastros e batidas sintéticos. O diretório fornecido veio sem metadados Git. A correspondência ao hash `59600163ccc9cffb8f3ae148beede7cd7866ae86` e o resultado na pista ficam [nao verificado]. `app/docs/LEIS.md:136` · `app/docs/LEIS.md:154`

As batidas estão em horário local UTC−3. As datas são de 2026 e cada caso é independente. No 6x1 comercial, o cadastro-base é de 44 h semanais, com 08:00–17:00 e pausa de 12:00–13:00 de segunda a sexta, sábado 08:00–12:00 e DSR no domingo. Esses horários são entradas da bateria. `app/docs/LEIS.md:75` · `app/docs/LEIS.md:107` · [nao verificado]

| ID | Família | Escala | CADASTRO | BATIDAS | RESPOSTA DA REGRA | FONTE |
|---|---|---|---|---|---|---|
| BAT-01 | BATIDA/ESPURIA | 6x1 comercial | Cadastro-base. O primeiro evento é retransmitido com a mesma identidade. | 23/11: 08:00:00 duas vezes, 12:00, 13:00, 17:00. | Normais **480**, HE50 0, HE100 0, noturno 0, falta 0, atraso 0. A retransmissão vale uma entrada. | L-087 `app/docs/LEIS.md:119`, L-099 `app/docs/LEIS.md:122`, L-110 `app/docs/LEIS.md:136`. |
| BAT-02 | BATIDA/ESPURIA | Personalizado sem intervalo | 08:00–12:00. As duas candidatas à entrada têm IDs distintos, sem confirmação de duplicidade. | 23/11: 08:04:58, 08:05:03, 12:00. | `lei: qual critério confirma duplicidade entre registros distintos, qual instante prevalece e como os segundos são comparados ao limite de tolerância?` **Pergunta 1.** | L-087 `app/docs/LEIS.md:119`, L-099 `app/docs/LEIS.md:122`. CLT, art. 58, § 1º. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art58) |
| BAT-03 | BATIDA/ESPURIA | 6x1 comercial | Cadastro-base. A batida isolada das 10:00 é suspeita de espúria, sem decisão ou retratação. | 23/11: 08:00, 10:00, 12:00, 13:00, 17:00. | `lei: qual evidência permite desconsiderar a marcação isolada na apuração e quais minutos são liberados enquanto a decisão está em aberto?` **Pergunta 2.** | L-025 `app/docs/LEIS.md:76`, L-087 `app/docs/LEIS.md:119`, L-088 `app/docs/LEIS.md:120`, L-125 `app/docs/LEIS.md:146`. |
| BAT-04 | BATIDA/ESPURIA | Personalizado sem intervalo | 08:00–12:00. Falta a saída. Colaborador sem resposta e sem outra evidência da saída. | 24/11: 08:00. | `lei: qual tratamento de pagamento e fechamento se aplica ao furo de saída sem resposta?` O marco segue em aberto. **Pergunta 3.** | L-030 `app/docs/LEIS.md:78`, L-039 `app/docs/LEIS.md:87`, L-088 `app/docs/LEIS.md:120`, L-103 `app/docs/LEIS.md:129`. |
| BAT-05 | BATIDA/ESPURIA | Personalizado sem intervalo | 08:00–12:00. Falta a entrada. Colaborador sem resposta e sem outra evidência da entrada. | 25/11: 12:00. | `lei: qual tratamento de pagamento e fechamento se aplica ao furo de entrada sem resposta?` O marco segue em aberto. **Pergunta 3.** | L-030 `app/docs/LEIS.md:78`, L-039 `app/docs/LEIS.md:87`, L-088 `app/docs/LEIS.md:120`, L-103 `app/docs/LEIS.md:129`. |
| BAT-06 | BATIDA/ESPURIA | Personalizado sem intervalo | 08:00–12:00. A marcação das 08:00 foi formalmente retratada. A entrada efetiva foi às 08:20. | 26/11: 08:00 retratada, 08:20, 12:00. | Normais **220**, HE50 0, HE100 0, noturno 0, falta 0, atraso **20**. O par apurável é 08:20–12:00. | L-025 `app/docs/LEIS.md:76`, L-084 `app/docs/LEIS.md:116`, L-087 `app/docs/LEIS.md:119`, L-093 `app/docs/LEIS.md:111`, `app/docs/GLOSSARIO.md:28`. |
| BAT-07 | BATIDA/ESPURIA | 12x36 noturno, 19:00–07:00 | CCT Vigilantes Londrina vigente e aplicada à empresa. Plantão previsto no dia 20, pausa de 21:00–22:00, folga no dia 21 e corte de competência no dia 21. | 20/10: 19:00, 21:00, 22:00. 21/10: 07:00. | No plantão de **20/10**, normais **660**, HE50 0, HE100 0, noturno **420** à alíquota de 20%, falta 0, atraso 0. A saída fecha o plantão na competência que termina no dia 20. O dia 21 recebe **0** minuto desse plantão. | L-026 `app/docs/LEIS.md:77`, L-085 `app/docs/LEIS.md:117`, CCT cl. 38-d e cl. 10 `CLAUDE.md:227` e `CLAUDE.md:236`, aplicação `CLAUDE.md:217`, parecer `app/core/regua_cct.py:142`. |
| BAT-08 | BATIDA/ESPURIA | 12x36 diurno, 07:00–19:00 | Plantão previsto, pausa de 12:00–13:00. A entrada de chão chega sem coordenadas. | 26/11: 07:00 sem GPS, 12:00, 13:00, 19:00. | A entrada é registrada e apurada. Normais **660**, HE50 0, HE100 0, noturno 0, falta 0, atraso 0. | L-030 `app/docs/LEIS.md:78`, L-087 `app/docs/LEIS.md:119`, L-032 `app/docs/LEIS.md:80`. |
| BAT-09 | BATIDA/ESPURIA | 6x1 comercial | Cadastro-base. Entrada efetiva seis minutos após o marco. | 26/11: 08:06, 12:00, 13:00, 17:00. | Normais **474**, HE50 0, HE100 0, noturno 0, falta 0, atraso **6**. O limite por marcação foi excedido. | L-084 `app/docs/LEIS.md:116`, L-093 `app/docs/LEIS.md:111`. CLT, art. 58, § 1º. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art58) |
| BAT-10 | BATIDA/ESPURIA | 6x1 comercial | Domingo de DSR previsto como folga. Trabalho autorizado e efetivo, sem folga compensatória ou banco de horas. | 29/11: 08:00, 12:00. | Normais 0, HE50 0, HE100 **240**, noturno 0, falta 0, atraso 0. O trabalho de domingo é pago em dobro e o DSR é preservado. | L-086 `app/docs/LEIS.md:118`, L-093 `app/docs/LEIS.md:111`, CLT, art. 67 e Súmula 146 do TST. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art67) · [TST](https://jurisprudencia-backend.tst.jus.br/rest/documentos/58ebcbe1221a82779995e18d8a4ba19d) |
| BAT-11 | BATIDA/ESPURIA | Personalizado sem intervalo em cada bloco, dupla pegada | Dois blocos previstos no mesmo dia, 08:00–12:00 e 16:00–20:00. Acordo escrito para o vão de quatro horas, integralmente de descanso. | 23/11: 08:00, 12:00, 16:00, 20:00. | Normais **480** = 240 + 240, HE50 0, HE100 0, noturno 0, falta 0, atraso 0. | L-024 `app/docs/LEIS.md:75`, L-087 `app/docs/LEIS.md:119`, L-091 `app/docs/LEIS.md:124`. CLT, art. 71, caput e § 2º. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art71) |
| BAT-12 | BATIDA/ESPURIA | Personalizado sem intervalo | Empresa com corte no dia 21. Dia 20 de folga, dia 21 com jornada prevista de 00:00–04:00. Dois cenários independentes, cada um com uma marcação avulsa e sem resposta do colaborador. | A: 20/10 às 23:58, nenhuma marcação em 21/10. B: 21/10 às 00:02, nenhuma marcação em 20/10. | `lei: enquanto a avulsa está sem par, qual competência assume a pendência em cada cenário e qual efeito ela tem no pagamento ou fechamento de cada competência?` **Pergunta 4.** | L-026 `app/docs/LEIS.md:77`, L-073 `app/docs/LEIS.md:100`, L-085 `app/docs/LEIS.md:117`, L-088 `app/docs/LEIS.md:120`, L-103 `app/docs/LEIS.md:129`. |
| BAT-13 | BATIDA/ESPURIA | Personalizado sem intervalo | 08:00–12:00. A saída das 11:40 foi formalmente retratada. Saída efetiva às 12:00. | 27/11: 08:00, 11:40 retratada, 12:00. | A exibição mostra **as três marcas cruas**. A apuração usa 08:00–12:00. Normais **240**, HE50 0, HE100 0, noturno 0, falta 0, atraso 0. | Portaria 671 conforme L-025 `app/docs/LEIS.md:76`, L-087 `app/docs/LEIS.md:119`, `app/docs/GLOSSARIO.md:28`. |
| BAT-14 | BATIDA/ESPURIA | Personalizado sem intervalo em cada bloco, dupla pegada | 08:00–12:00 e 16:00–20:00, com acordo escrito para o vão de quatro horas. Teste do limite diário com variações de até cinco minutos por marcação. | 24/11: 08:05, 12:00, 16:05, 20:00. 25/11: 08:05, 12:00, 16:05, 19:59. | **24/11**, soma de desvios **10**: normais **480**, HE50 0, HE100 0, noturno 0, falta 0, atraso 0, saída antecipada 0. **25/11**, soma **11**: normais **469**, HE50 0, HE100 0, noturno 0, falta 0, atraso **10**, saída antecipada **1**. | CLT, art. 58, § 1º e art. 71, caput. L-093 `app/docs/LEIS.md:111`. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art58) |
| BAT-15 | BATIDA/ESPURIA | 6x1 comercial | Cadastro-base, pausa fechada. Trabalho até 18:31 comprovado e os 91 minutos após o marco expressamente autorizados para pagamento, sem compensação. | 26/11: 08:00, 12:00, 13:00, 18:31, com a última marcação gravada como `E`. | A última marcação é a saída pela posição, **91 min** após o marco de 17:00. Normais **480**, HE50 **91**, HE100 0, noturno 0, falta 0, atraso 0. | L-087 `app/docs/LEIS.md:119`, L-097 `app/docs/LEIS.md:115`, L-115 `app/docs/LEIS.md:153`. CLT, art. 59, § 1º. [CLT compilada](https://planalto.gov.br/ccivil_03/decreto-lei/del5452compilado.htm#art59) |

### Propriedades fixas da família

- **Registro e apuração.** A batida de chão entra. A exibição preserva o cru. A apuração exclui a marca retratada e atribui o papel pelo instante contra os marcos cadastrados. `app/docs/LEIS.md:76` · `app/docs/LEIS.md:78` · `app/docs/LEIS.md:119`
- **Furo e falta.** Marco faltante permanece em aberto. A falta exige decisão. O zero de realizado para o fechamento sem par tem o alcance da L-103. A apresentação `Sem turno pareado` segue a L-102. `app/docs/LEIS.md:87` · `app/docs/LEIS.md:120` · `app/docs/LEIS.md:128` · `app/docs/LEIS.md:129`
- **Virada do dia.** A jornada pertence ao dia de início. A competência usa o corte da empresa. A saída após a meia-noite fecha o plantão de origem. `app/docs/LEIS.md:77` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:123`
- **Coerência dos minutos.** Repetir o mesmo fato dá o mesmo resultado, o previsto vem do cadastro, as parcelas de trabalho somam o total e os leitores usam o mesmo valor. O noturno é a base do adicional sobre os minutos elegíveis. `app/docs/LEIS.md:60` · `app/docs/LEIS.md:75` · `app/docs/LEIS.md:113` · `app/docs/LEIS.md:122` · `app/docs/LEIS.md:136` · `app/core/regua_cct.py:136`

### Perguntas de lei

1. **BAT-02.** Quais critérios confirmam que duas marcas com IDs distintos e poucos segundos de diferença representam uma só batida? Qual instante prevalece e qual precisão se aplica ao limite de cinco minutos? `app/docs/LEIS.md:119` · `app/docs/LEIS.md:122` · [nao verificado]
2. **BAT-03.** Qual evidência permite excluir da apuração uma candidata a espúria isolada, sem retratação, e quais minutos podem ser pagos enquanto ela aguarda decisão? `app/docs/LEIS.md:76` · `app/docs/LEIS.md:120` · `app/docs/LEIS.md:146` · [nao verificado]
3. **BAT-04 e BAT-05.** Qual pagamento ou retenção se aplica ao furo de entrada ou saída sem resposta do colaborador no fechamento? Que evidência pode resolver cada ponta? `app/docs/LEIS.md:78` · `app/docs/LEIS.md:120` · `app/docs/LEIS.md:129` · [nao verificado]
4. **BAT-12.** A qual competência pertence a pendência de uma avulsa junto ao corte 20/21 antes de haver par, e qual é seu efeito em cada fechamento? `app/docs/LEIS.md:77` · `app/docs/LEIS.md:117` · `app/docs/LEIS.md:129` · [nao verificado]

## 27/11 15:05 · sessao 3acafa1bd4ec · INVENTARIO-PELA-REGRA · leitura · origem=Code

## Vermelhos

Os valores observados vêm do `bateria.log` fornecido. As linhas abaixo identificam as asserções e os caminhos de produção na cópia. Usei a L-099 para atribuir o dono ao dado, ao código de produção ou, quando a asserção mede outra coisa, ao instrumento. [app/docs/LEIS.md:122] [app/docs/LEIS.md:136]

| Caso | Regra | Sistema | Dono | Sítio | Nota |
|---|---|---|---|---|---|
| **ESC-11** · `campo='horas_extra_50'` | CLT art. 59 e L-097/L-115. Os **150 min autorizados** contam integralmente após a saída posicional. O teste pede **150 em HE50**. O corte HE100-RUBRICA do projeto destina o excedente diário acima de 120 min à HE100 de dia normal. [app/docs/LEIS.md:115] [app/docs/LEIS.md:153] [app/docs/DOSSIES.md:189] [app/docs/RELATO-ARQUIVO.md:16815] [app/ponto/tests/test_inventario_escala.py:327] | **120 min em HE50**, conforme o log. A divisão do motor destina os outros **30 min à HE100**. Essa segunda parcela é inferida do código. [app/ponto/tests/test_inventario_escala.py:171] [app/ponto/motor_calculo_v2.py:3526–3540] | **INSTRUMENTO** | `MotorComercial.calcular_mes` divide a extra diária entre HE50 e HE100. [app/ponto/motor_calculo_v2.py:3533–3540] | Medir 630 trabalhados, 150 de extra total, 120 em HE50 e 30 em HE100 de dia normal. Assim a asserção cobra o reconhecimento integral da autorização e a rubrica definida pelo corte. [app/ponto/tests/test_inventario_escala.py:321–327] [app/docs/RELATO-ARQUIVO.md:16815] | [nao verificado]
| **ESC-11** · asserção do excedente | O trabalho autorizado excede o limite ordinário de 120 min em **30 min**. O teste procura esse excedente em `HE50 − teto`. [app/core/regua_cct.py:50] [app/ponto/tests/test_inventario_escala.py:328–330] | **0 min** na expressão testada, pois ela subtrai 120 dos 120 classificados como HE50. [app/ponto/tests/test_inventario_escala.py:330] [app/ponto/motor_calculo_v2.py:3533–3539] | **INSTRUMENTO** | `MotorComercial.calcular_mes` e os agregadores de HE50 e HE100. [app/ponto/motor_calculo_v2.py:323–336] [app/ponto/motor_calculo_v2.py:3533–3539] | Comparar **HE total − teto = 30** e conferir os 30 na HE100 de dia normal. [app/docs/RELATO-ARQUIVO.md:16815] [app/ponto/motor_calculo_v2.py:323–336] | [nao verificado]
| **ESC-12** | CLT art. 67, Lei 605/49, Súmula 146 e L-086. No domingo de DSR trabalhado e sem compensação, **0 previsto, 240 trabalhados como base da dobra, 0 normais e 0 HE50**. [app/docs/LEIS.md:118] [app/ponto/tests/test_inventario_escala.py:332–349] | O previsto **0** passou. A soma `horas_trabalhadas + horas_ft` deu **0 min**. A rubrica na lavra ficou [nao verificado], pois a falha ocorre antes de `fechar()`. [app/ponto/tests/test_inventario_escala.py:344–349] | **ESTRUTURA** | `parear_turnos` usa a pausa do template quando o domingo vem sem marcos no DNA. O ramo de absorção toma `12:00 S` como pausa e o fecho posterior exige uma batida depois dela. [app/ponto/management/commands/gerar_celulas.py:73–93] [app/ponto/turnos.py:1224–1244] [app/ponto/turnos.py:1356–1376] [app/ponto/turnos.py:1166–1175] | A leitura do código explica o zero no recorte. Há um par `08:00 E / 12:00 S` na massa e o motor comercial devolve zero trabalhado para o período que recebe sem saída. **Single Source of Truth / Aggregate Root**. O pareamento usa marcos do template para uma folga que a célula declara sem marcos. [app/ponto/tests/test_inventario_escala.py:123–135] [app/ponto/tests/test_inventario_escala.py:334–346] [app/ponto/motor_calculo_v2.py:3331–3336] [app/docs/GLOSSARIO.md:11] | [nao verificado]
| **AUS-03** | L-022/L-089. O atestado cobre só o dia de folga do 12×36. O dia mantém **`folga`, 0 previsto, 0 abono, 0 falta e 0 consumo de saldo**. [app/docs/LEIS.md:73] [app/docs/LEIS.md:121] [app/ponto/tests/test_inventario_ausencia.py:61–75] | **`('ausencia', 0)`**. Abono, falta e saldo nesta rodada [nao verificado]. A asserção de classificação interrompe as duas conferências seguintes, e a de saldo é um skip. [app/ponto/tests/test_inventario_ausencia.py:70–75] | **ESTRUTURA** | `_marcar_ausencias` preserva a folga para `falta`, mas aplica o rótulo `ausencia` ao atestado. O cartório copia esse tipo para a ata, e `grade_da_celula` o lê. [app/escala/utils.py:1248–1265] [app/ponto/services/cartorio.py:749–759] [app/escala/services/leitor_celula.py:397–422] | É um erro de precedência na escrita da projeção da grade, documentado na dívida da L-089. Quebra adicional de padrão arquitetural [nao verificado]. [app/docs/LEIS.md:180] [app/docs/GLOSSARIO.md:25] |
| **AUS-05** · `ERROR` | L-039/L-088. Uma entrada às 08:00 com pergunta em aberto preserva **480 previstos** e o **furo em aberto**, sem falta decidida nem desconto de DSR por esse evento. [app/docs/LEIS.md:87] [app/docs/LEIS.md:120] [app/ponto/tests/test_inventario_ausencia.py:90–118] | A criação da pergunta levantou `ValueError`: a fixture passou `instancia_marco='fim'`, e `batida_ausente` deriva `''`. Resultado da apuração [nao verificado]. [app/ponto/tests/test_inventario_ausencia.py:103–111] [app/chamados/models.py:3898–3909] | **INSTRUMENTO** | `PerguntaDisputa.save` valida a identidade no nascimento. `instancia_do_motivo` deriva a posição pela tabela de motivos. [app/chamados/models.py:3852–3856] [app/chamados/services/declaracao_texto.py:43–58] | Para a pergunta genérica `batida_ausente`, omitir `instancia_marco` e deixar o modelo derivá-la. Para uma pergunta específica sobre a saída, usar o motivo de marco `orfao_14h`, que deriva `fim`. Manter a pergunta aberta e as asserções de previsto, furo e falta. [app/chamados/models.py:3898–3909] [app/chamados/services/declaracao_texto.py:43–58] [app/ponto/tests/test_inventario_ausencia.py:113–118] | [nao verificado]
| **FOL-03** · `rubrica='9902'` | Lei 605/49, Súmula 146 e L-086/L-095. O DSR trabalhado deve contribuir **240 min à base da dobra**, com **0 HE50** e **0 HE100 de feriado** no delta. `9902` é o código escolhido pela fixture para observar essa parcela. [app/docs/LEIS.md:113] [app/docs/LEIS.md:118] [app/folha/tests/test_inventario_folha.py:33–39] [app/folha/tests/test_inventario_folha.py:155–165] | O delta lido em **`9902` foi 0**. O valor efetivamente apurado em `horas_folga_trabalhada` nessa rodada fica [nao verificado]. [app/folha/tests/test_inventario_folha.py:95–104] [app/folha/tests/test_inventario_folha.py:164–165] | **INSTRUMENTO** | A fixture vincula `9902` a `dsr_trabalhado_diurno`, uma opção do catálogo. O emissor lê `horas_folga_trabalhada` e emite **`folga_trabalhada_diurna`**. [app/folha/tests/test_inventario_folha.py:52–68] [app/folha/models.py:32–34] [app/folha/export.py:439–447] [app/folha/export.py:739–748] | Mapear e homologar `9902` para o evento efetivamente emitido. Conferir também os 240 min de folga trabalhada no fechamento antes de comparar cenário e controle no TXT. Isso verifica se a apuração entregou a base ao emissor. [app/ponto/services/fechamento.py:953–990] [app/folha/export.py:993–1000] [app/folha/tests/test_inventario_folha.py:160–165] |
| **FOL-13** | L-099/L-114. Duas gerações com as mesmas entradas devem produzir **os mesmos bytes e totais por rubrica**. [app/docs/LEIS.md:122] [app/docs/LEIS.md:154] [app/folha/tests/test_inventario_folha.py:307–321] | O primeiro download esperado deu **204**. O log registra duas recusas por falta de eventos. A comparação de bytes e totais ficou [nao verificado]. [app/folha/tests/test_inventario_folha.py:117–128] [app/folha/tests/test_inventario_folha.py:314–321] [app/folha/views.py:275–280] | **INSTRUMENTO** | O POST chama a rota de geração. A massa de jornada ordinária depende de um evento de normais que o emissor não oferece. Sem linhas, a resposta HTMX é `204` com mensagem de erro. [app/folha/urls.py:14] [app/folha/export.py:439–461] [app/folha/export.py:739–755] [app/core/respostas.py:63–66] | Incluir na massa um evento não zero que a rota emita, por exemplo **60 min de HE autorizada e homologada**. Manter as duas chamadas idênticas, exigir dois downloads e comparar bytes e agregados. [app/folha/tests/test_inventario_folha.py:52–68] [app/folha/tests/test_inventario_folha.py:144–153] [app/folha/tests/test_inventario_folha.py:314–321] |
| **FOL-14** · `etapa='reemissão'` | L-093/L-113. Fora da guarda, a correção leva o recorte de **450 para 480 min trabalhados** e de **30 para 0 de atraso**. A emissão corrigida deve substituir a anterior. [app/docs/LEIS.md:111] [app/docs/LEIS.md:155–156] [app/ponto/services/fechamento.py:36–50] [app/folha/tests/test_inventario_folha.py:343–373] | A primeira emissão levou **30 min de atraso**. A lavra corrigida passou com **480 trabalhados e 0 atraso**, mas o segundo POST devolveu **204, “Nenhum evento a exportar”**. [app/folha/tests/test_inventario_folha.py:343–348] [app/folha/tests/test_inventario_folha.py:368–373] [app/folha/views.py:275–280] | **ESTRUTURA** | O emissor não oferece evento de normais, descarta eventos de valor zero e recusa a geração sem linhas antes de registrar o TXT. [app/folha/export.py:439–461] [app/folha/export.py:739–755] [app/folha/views.py:275–307] | Neste recorte, corrigir o atraso remove a única linha exportável da massa. **CQRS com projeções reconstruíveis**. A lavra incorpora a correção, mas a emissão fica sem uma versão que a represente. O leiaute para esse resultado fica [nao verificado]. [app/folha/tests/test_inventario_folha.py:325–326] [app/folha/tests/test_inventario_folha.py:341–373] [app/docs/GLOSSARIO.md:25] | [nao verificado]
| **FOL-14** · `etapa='vigência'` | L-113. Um TXT vigente para a empresa e competência corrigidas, com a versão anterior intacta no histórico e preservação dos dois controles. [app/docs/LEIS.md:155–156] [app/folha/tests/test_inventario_folha.py:331–339] [app/folha/tests/test_inventario_folha.py:375–384] | **`vigente.pk == antigo.pk == 3`**. As conferências posteriores de conteúdo e hash do antigo e dos controles passaram até a asserção do aviso. [app/folha/tests/test_inventario_folha.py:375–385] | **ESTRUTURA** | A recusa ocorre antes de `ExportacaoDominio.registrar`. A vigência lê o registro mais recente da mesma empresa, mês e ano. [app/folha/views.py:277–280] [app/folha/views.py:302–307] [app/folha/models.py:174–188] [app/folha/export.py:589–603] | Mesmo desvio da reemissão, observado na vigência. **CQRS com projeções reconstruíveis**. O leitor de vigência devolve o único resultado que o fluxo deixou disponível para a tripla. [app/docs/GLOSSARIO.md:25] [app/folha/export.py:589–603] |
| **FOL-14** · aviso ao DP | L-113 × DINHEIRO 2. Uma exportada que muda fora da guarda gera **aviso ao DP com os dois números**. [app/docs/LEIS.md:156] [app/folha/tests/test_inventario_folha.py:350–369] | A consulta de novas pautas para o DP devolveu **`False`**. O recálculo produziu o aviso de log sobre a competência exportada. [app/folha/tests/test_inventario_folha.py:350–355] [app/folha/tests/test_inventario_folha.py:385] [app/ponto/services/fechamento.py:559–567] | **ESTRUTURA** | O caminho genérico agenda e executa o recálculo e registra logs. O ramo da exportada registra o `warning` sem entregar os dois valores ao escritor de Pauta. A âncora de competência buscada pelo teste coincide com a forma do modelo. [app/ponto/services/fechamento.py:1822–1848] [app/ponto/services/fechamento.py:559–567] [app/pautas/models.py:74–89] | **Event-driven architecture**. O evento chega ao recálculo, e o aviso ao DP exigido para a alteração fica ausente nesse caminho. [app/docs/GLOSSARIO.md:16] [app/docs/LEIS.md:156] [app/ponto/services/fechamento.py:559–567] | [nao verificado]

## 38 skips

A cópia declara **39** chamadas a `sem_campo`. A de **AUS-05** fica depois do `ValueError`, o que deixa **38 skips executados**: 13 de escala, 9 de ausência e 16 de folha. O texto completo tem a forma `sem campo: <campo>. <motivo>`. A tabela agrupa pelo `<motivo>` e identifica cada ocorrência pela chamada. [app/ponto/tests/test_inventario_escala.py:173–175] [app/ponto/tests/test_inventario_ausencia.py:103–110]

As referências **ESC**, **AUS** e **FOL** na tabela apontam, respectivamente, para `app/ponto/tests/test_inventario_escala.py`, `app/ponto/tests/test_inventario_ausencia.py` e `app/folha/tests/test_inventario_folha.py`. [app/ponto/tests/test_inventario_escala.py:178–180] [app/ponto/tests/test_inventario_ausencia.py:31] [app/folha/tests/test_inventario_folha.py:47–48]

| Motivo literal do skip | Ocorrências | Classe |
|---|---|---|
| A apuração não tem parcela exclusiva de normais. | **9** · ESC-01:190, ESC-02:207, ESC-04:244, ESC-06:283, ESC-07:297, ESC-10:311, ESC-11:324, ESC-15:375, AUS-08:162 | ESTRUTURA-sem-campo |
| DiaPago não tem parcela exclusiva de normais. | **4** · ESC-03:226, ESC-13:360, AUS-04:84, AUS-10:187 | ESTRUTURA-sem-campo |
| O emissor não tem evento de normais. | **8** · FOL-01:136, FOL-02:151, FOL-04:175, FOL-05:186, FOL-09:242, FOL-11:281, FOL-12:298, FOL-14:341 | ESTRUTURA-sem-campo |
| O vínculo de quatro horas não declara regime de remuneração. | **4** · AUS-01:40, FOL-06:196, FOL-07:210, FOL-08:227 | ESTRUTURA-sem-campo |
| A lavra diária não contém desconto de DSR. | **2** · AUS-02:55, AUS-10:186 | ESTRUTURA-sem-campo |
| A grade classifica o domingo como folga. | **1** · ESC-01:189 | ESTRUTURA-sem-campo |
| O período expõe trabalhado total e noturno de relógio. | **1** · ESC-05:262 | ESTRUTURA-sem-campo |
| DiaPago não distingue essas parcelas. | **1** · ESC-12:343, para normais e HE100 específica de DSR | ESTRUTURA-sem-campo |
| A apuração expõe semanas com DSR e reflexo, sem essas duas bases. | **1** · AUS-01:41 | ESTRUTURA-sem-campo |
| O atestado não expõe consumo de saldo. | **1** · AUS-03:70 | ESTRUTURA-sem-campo |
| O agendamento expõe gozo e dias. A apuração de ponto não expõe verbas nem pagamento de férias. | **1** · AUS-06:131–132 | ESTRUTURA-sem-campo |
| O emissor não publica normais nem as duas bases de DSR. | **1** · FOL-06:197–198 | ESTRUTURA-sem-campo |
| O TXT registra faltas em dias, com as datas em registros 11. | **1** · FOL-07:211. Os minutos são consultados separadamente. [app/folha/tests/test_inventario_folha.py:213–215] [app/folha/export.py:215–221] | ESTRUTURA-sem-campo |
| O emissor não publica essas duas bases em minutos. | **1** · FOL-08:228, para DSR devido e descontado | ESTRUTURA-sem-campo |
| O emissor não tem evento de normais e a lavra não tem essa parcela exclusiva. | **1** · FOL-10:260–261 | ESTRUTURA-sem-campo |
| Pauta expõe texto livre, sem campos de valores por rubrica. | **1** · FOL-14:342. É o skip do **formato estruturado**. A existência do aviso foi testada e falhou. [app/pautas/models.py:83–89] [app/folha/tests/test_inventario_folha.py:385] | ESTRUTURA-sem-campo |

As sete **perguntas de lei** estão no `bateria.md` fornecido e ficaram fora dos 35 métodos e dos 38 skips. São **ESC-08/Q1** rubrica e percentual da folga comum trabalhada, **ESC-09/Q2** tratamento dos 60 min de interjornada, **ESC-14/Q3** vigência e rubrica da dobra histórica, **AUS-09/Q4** efeitos do afastamento previdenciário, **AUS-11/Q5** remuneração e DSR na licença particular, **AUS-12/Q6** férias e terço atravessando o corte, e **FOL-15/Q7** contrato de leiaute do TXT. [app/ponto/tests/test_inventario_escala.py:297–305] [app/ponto/tests/test_inventario_escala.py:351–369] [app/ponto/tests/test_inventario_ausencia.py:153–172] [app/ponto/tests/test_inventario_ausencia.py:194] [app/folha/tests/test_inventario_folha.py:385]

## 27/11 14:57 · sessao 1ba509958f06 · INVENTARIO-PELA-REGRA · construtor · origem=Code

Implementei os **35 casos de medida** em três arquivos novos sobre o HEAD `59600163ccc9cffb8f3ae148beede7cd7866ae86`. Conferi a sintaxe, os IDs e as docstrings de fonte. O SHA é o da base. A proibição de escrita em `.git` impediu gerar um SHA de commit para a entrega. [Escala](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:180) · [Ausência](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:31) · [Folha](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:130)

**Placar Django [nao verificado].** O Python local não dispõe de `django.test` nem de `freezegun`, e o recorte da suíte usa SSH e referências Git. O candidato a RED é **FOL-03**. Ele cobra 240 minutos no evento cadastrado `dsr_trabalhado_diurno`. A lista do emissor passa diretamente de feriado para folga trabalhada. [FOL-03](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:155) · [Emissor](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/export.py:439) · [Rota da suíte](/home/ronald/wt-codex-inventario-pela-regra-143921/bin/suite.sh:320) [nao verificado]

### Recibo por caso

**Grade** é `grade_da_celula`, **motor** é `autoridade_do_periodo`, **fechamento** é `recalcular_fechamento_mes`, **TXT** é `montar_linhas_export` sobre `fechamento_lido`, e **POST** é `gerar_dominio_txt`. Os números abaixo são gabaritos em minutos, salvo a unidade indicada. [Grade](/home/ronald/wt-codex-inventario-pela-regra-143921/app/escala/services/leitor_celula.py:350) · [Motor](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/services/espelho.py:1007) · [Fechamento](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/services/fechamento.py:468) · [TXT](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:86) · [POST](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/views.py:104)

| Caso | Porta | Campo lido e gabarito |
|---|---|---|
| [ESC-01](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:180) | Grade, motor | Previsto `480 × 5 / 240 / 0`, total previsto e trabalhado `2.640`, domingo `folga`, HE50 e HE100 `0` |
| [ESC-02](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:199) | Grade, motor | Previsto `660 / 0 / 660`, trabalhado nos dois plantões `660 / 660`, descanso entre saída e entrada `2.160` |
| [ESC-03](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:215) | Grade, fechamento | Previsto `480 / 660 / 0`, vínculo gerador anterior em 03/11 e atual em 04–05/11, trabalhado `480 / 660`, 05/11 `folga` |
| [ESC-04](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:237) | Grade, motor, `regua_para` | Previsto `660 / 0`, um turno de `10/11 19:00` a `11/11 07:00`, trabalhado `660`, base noturna `420` a `20%` |
| [ESC-05](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:255) | Grade, motor, `regua_para` | Previsto e trabalhado `660`, noturno de relógio `420`, base legal `480` a `20%`, adicional após 05:00 `0` |
| [ESC-06](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:275) | Grade, motor | Previsto e trabalhado `660`, HE100 feriado `0` |
| [ESC-07](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:288) | Grade, motor | 20/11 `folga`, previsto, trabalhado, falta e HE100 feriado `0` |
| [ESC-10](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:303) | Grade, motor | Previsto `480`, trabalhado `540`, HE50 `60`, HE100 `0` |
| [ESC-11](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:316) | Grade, motor, `regua_para` | Previsto `480`, trabalhado `630`, HE50 `150`, limite ordinário `120`, excesso `30` |
| [ESC-12](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:332) | Grade, motor, fechamento | Previsto `0`, trabalhado `240`, HE50 `0`, HE100 total `240` |
| [ESC-13](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:351) | Grade, motor, fechamento | Previsto `0`, trabalhado `480`, HE50 `0`, HE100 feriado `480` |
| [ESC-15](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:367) | Grade, motor, `piso_intervalo_art71` | Previsto `240 / 240`, trabalhado `240 / 210`, antecipada `0 / 30`, HE e piso de intervalo `0` |
| [AUS-01](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:31) | Fechamento, motor | Falta decidida `240` em 29/10, semanas com DSR `0` |
| [AUS-02](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:48) | Fechamento | Previsto e abono `480`, trabalhado e falta `0` |
| [AUS-03](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:61) | Grade, fechamento | 10/11 `folga`, previsto, abono e falta `0` |
| [AUS-04](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:77) | Fechamento | Previsto `480`, trabalhado `450`, atraso `30`, antecipada e falta integral `0` |
| [AUS-05](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:90) | Fechamento, pergunta persistida | Previsto `480`, `furo_apurado=True`, pergunta sem resposta ou validação, falta `0` |
| [AUS-06](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:120) | `registrar_gozo_historico`, motor | Agendamento de `30 dias`, falta e atraso `0` |
| [AUS-07](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:137) | `registrar_gozo_historico`, `ausencias_na_janela`, fechamento | Um agendamento de `30 dias`, `12 / 18 dias` de gozo nos recortes, falta `0 / 0` |
| [AUS-08](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:153) | `em_gozo_hoje`, motor | `30 dias`, 12/10 dentro do gozo, 28/10 fora, trabalhado no retorno `480`, HE100 feriado no gozo `0` |
| [AUS-10](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:171) | Fechamento | Abono `480 / 480 / 480`, total `1.440`, falta `0`, trabalhado no retorno `480` |
| [FOL-01](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:130) | TXT, fechamento | Trabalhado `480`; HE50, HE100, noturno, falta e atraso `0` |
| [FOL-02](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:144) | TXT | HE50 `60`, HE100 `0` |
| [FOL-03](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:155) | TXT | HE100 de DSR `240`, HE50 e HE100 feriado `0` |
| [FOL-04](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:167) | TXT | HE100 feriado `480`, HE100 de DSR e HE50 `0` |
| [FOL-05](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:179) | TXT, fechamento, `regua_para` | Trabalhado `660`, `0025` com base `420`, alíquota `20%` no cadastro |
| [FOL-06](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:192) | TXT, motor | Trabalhado na semana `1.440`, uma semana com direito a DSR |
| [FOL-07](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:204) | TXT, `minutos_de_falta_por_dia` | Falta `240` na fonte, `1 dia` em `8792` com data `20261029`, atraso `0` |
| [FOL-08](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:223) | TXT, motor, `minutos_de_falta_por_dia` | Falta `240` na fonte e `1 dia` na rubrica de 29/10, semanas com direito a DSR `0` |
| [FOL-09](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:236) | TXT, fechamento | Atraso `30`, trabalhado `450`, falta integral e antecipada `0` |
| [FOL-10](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:248) | TXT, fechamento, `pago_do_dia` | HE50 `60`, HE100 feriado `480`, atraso `30`; trabalhado `480 + 540 + 450 + 480 = 1.950` |
| [FOL-11](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:270) | TXT, `janela_competencia`, fechamento | Janela `21/10–20/11`, HE100 feriado `480` em novembro, 20/10 e 21/11 nas competências vizinhas |
| [FOL-12](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:291) | TXT, fechamento | Outubro recebe trabalhado `660` e `0025` de `420`. Novembro recebe `0` desse plantão |
| [FOL-13](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:307) | POST, `agregar_export_dominio` | Dois downloads com bytes e totais por rubrica iguais |
| [FOL-14](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:323) | POST, fechamento, `competencia_entregue` | Trabalhado `450 → 480`, atraso `30 → 0`, um TXT vigente, bytes e hash anteriores preservados, dois controles estáveis e nova pauta ao DP |

### `lei:` pendentes

`ESC-08` Q1, folga comum trabalhada. `ESC-09` Q2, interjornada. `ESC-14` Q3, feriado histórico. `AUS-09` Q4, afastamento previdenciário. `AUS-11` Q5, licença particular. `AUS-12` Q6, apropriação de férias no corte. `FOL-15` Q7, contrato do leiaute. [Bateria anexa, seção «Perguntas de lei», Q1–Q7]

### Sem campo

Os skips são explícitos por subteste. As demais verificações do caso continuam executáveis. Os códigos `9901` e `9902` são exclusivos da fixture para distinguir feriado de DSR trabalhado. [Skip](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:173) · [Códigos da fixture](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:32)

- **sem campo: normais como parcela exclusiva** em ESC-01–04, 06–07, 10–13 e 15, AUS-04, 08 e 10, e FOL-01, 02, 04–06, 09–12 e 14. [Campos da lavra](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/models.py:324) · [Eventos do TXT](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/export.py:439)
- **sem campo: classificação DSR** em ESC-01, **HE100 diária específica de DSR** em ESC-12 e **240 minutos diurnos de relógio** em ESC-05. [Grade](/home/ronald/wt-codex-inventario-pela-regra-143921/app/escala/services/leitor_celula.py:397) · [Lavra](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/models.py:324) · [Resultado do período](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/motor_calculo_v2.py:205)
- **sem campo: remuneração horista em vínculo 6×1** em AUS-01 e FOL-06–08. **sem campo: DSR devido e descontado em minutos** em AUS-01, 02, 05 e 10 e FOL-06 e 08. [Cadastro da escala](/home/ronald/wt-codex-inventario-pela-regra-143921/app/escala/models.py:816) · [Resultado mensal](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/motor_calculo_v2.py:270) · [Emissor](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/export.py:453)
- **sem campo: consumo de saldo por atestado**, AUS-03. **sem campo: férias brutas R$ 3.000, terço R$ 1.000, total R$ 4.000 e pagamento até 28/02**, AUS-06. [AUS-03](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:70) · [AUS-06](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:131)
- **sem campo: 240 minutos na linha TXT de falta**, FOL-07. A fonte informa minutos e o TXT informa dias e datas. **sem campo: valores estruturados de antes/depois no aviso ao DP**, FOL-14. A pauta tem texto livre. [Falta no TXT](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/export.py:215) · [Pauta](/home/ronald/wt-codex-inventario-pela-regra-143921/app/pautas/models.py:86)

O instrumento escrito do 12×36 CLT ficou como premissa da massa [nao verificado]. A fixture cadastra a fase e o horário. A empresa B também tem aplicação de CCT vinculada à praça. [Fixtures](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:47)

### Comandos para o Code

```bash
PYTHONDONTWRITEBYTECODE=1 bash bin/suite.sh \
  --dir /home/ronald/wt-codex-inventario-pela-regra-143921 \
  --only "ponto.tests.test_inventario_escala ponto.tests.test_inventario_ausencia folha.tests.test_inventario_folha" \
  --parallel 1
```

Os três comandos de mutação abaixo partem de `app/` no ambiente de teste. Cada um altera uma resposta em memória e executa só o caso correspondente. Resultado dos mutantes [nao verificado]. [ESC-15](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_escala.py:367) · [AUS-07](/home/ronald/wt-codex-inventario-pela-regra-143921/app/ponto/tests/test_inventario_ausencia.py:137) · [FOL-02](/home/ronald/wt-codex-inventario-pela-regra-143921/app/folha/tests/test_inventario_folha.py:144) [nao verificado]

```bash
mutante_inventario() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python - "$1" <<'PY'
import sys
from datetime import date
from importlib import import_module
from unittest.mock import patch

import django
from django.core.management import call_command

django.setup()

modo = sys.argv[1]
alvos = {
    'intervalo': (
        'ponto.tests.test_inventario_escala.piso_intervalo_art71',
        'ponto.tests.test_inventario_escala.InventarioEscalaTest.test_ESC_15_quatro_horas_e_saida_antecipada',
    ),
    'gozo': (
        'ponto.tests.test_inventario_ausencia.ausencias_na_janela',
        'ponto.tests.test_inventario_ausencia.InventarioAusenciaTest.test_AUS_07_gozo_continuo_no_corte_do_dia_21',
    ),
    'he50': (
        'folha.export.eventos_do_fechamento',
        'folha.tests.test_inventario_folha.InventarioFolhaTest.test_FOL_02_extra_autorizada_no_txt',
    ),
}
alvo, teste = alvos[modo] [nao verificado]
modulo, nome = alvo.rsplit('.', 1)
original = getattr(import_module(modulo), nome)

def mutado(*args, **kwargs):
    if modo == 'intervalo':
        return 15
    resultado = original(*args, **kwargs)
    if modo == 'gozo':
        return resultado.exclude(data=date(2026, 11, 20))
    return [(evento, valor) for evento, valor in resultado if evento != 'hora_extra_diurna']

with patch(alvo, side_effect=mutado):
    call_command('test', teste, interactive=False, parallel=1) [nao verificado]
PY
}

mutante_inventario intervalo
mutante_inventario gozo
mutante_inventario he50
```

## 27/11 14:48 · sessao 7b6055d82c76 · PAR-PELO-TURNO · construtor · origem=Code

**PAR-PELO-TURNO c1v13** está na cópia sobre o HEAD `74b4f07168adf5a27e35266e7175cc31bf952834`. A proibição de `git commit` e de escrita em `.git*` prevaleceu. O heredoc com `OBRA` e `LEI-AKITA` está no recibo. [SHA e comando de commit](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:181)

**L-073, contestável no RELATO.** Às 02:00, espelho e cartão exibem as duas marcas, com **0 minutos fechados, 0 h de relógio e 1 turno aberto**, sem publicar “Em aberto”. Às 14:00, a saída entra no corte inclusivo e os dois registram **960 minutos e geometria fechada**. A folha usa o instante da borda, e `montar_dias` é o escritor de `datas_saida_orfa` para a janela com linhas, inclusive quando o PDF recebe `_turnos_juiz=[]`. [RELATO](/home/ronald/wt-codex-par-pelo-turno-111717/app/docs/RELATO.md:3) · [REDs V1–V3](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/test_par_pelo_turno_c1v13.py:47) · [Relação `arquivo::função`](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:140)

Os REDs deram **5 falhas de asserção antes da cura**. Depois, **73 testes passaram em memória** e cada um dos três mutantes falhou na asserção esperada. A contagem de `except` adicionado foi **0**. [Log anterior à cura](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:5) · [Comandos e resultados dos mutantes](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:111) · [Contagem](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:180) [nao verificado]

### Chamadores com `ate=None`

Na segunda virada, a autoridade passou de `[1]`, 0 h e 1 aberto no main para `[1,2]`, 26 h e 0 abertos na obra. O main do `porta_export` já alcançava o par pedindo `fim + 1 dia`. [Comparação reproduzida](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:149) [nao verificado]

| Chamador | Classificação no caso |
|---|---|
| `calendario` | Cura pretendida na carga. A lista de faltas consumida ficou `[]` nas duas versões. [Chamada](/home/ronald/wt-codex-par-pelo-turno-111717/app/colaboradores/services/calendario.py:602) |
| `porta_export` | Cura pretendida no recorte. As duas janelas deram 26 h para o turno. [Comparação](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:159) |
| `aplicar_09_he_intra` | Cura pretendida na carga dos dois lados da medição. Efeito financeiro [nao verificado]. [Chamada](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/management/commands/aplicar_09_he_intra.py:86) |
| `autorizacao_he_periodo` | Cura pretendida na carga da medição. Efeito na autorização [nao verificado]. [Chamada](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/services/autorizacao_he_periodo.py:88) |
| `diff_janela_he` | Cura pretendida na carga. Delta das rubricas [nao verificado]. [Chamada](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/management/commands/diff_janela_he.py:58) |
| `diff_calculador` | Cura pretendida na carga do lado da autoridade. Delta agregado [nao verificado]. [Chamada](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/management/commands/diff_calculador.py:145) |

**Pendente para o Code e o opus final.** O comando com todos os labels está no recibo. O `suite.sh` usa SSH ou Docker nesse recorte, ambos vedados nesta volta. Suite de integração, PDF binário, banco e produção [nao verificado]. Ficaram fora a exibição de retratadas pela L-025 e a apuração própria dos cards do app. O selo vizinho `test_falta_um_significado.py:177` falhou em memória porque procura a chamada literal com quatro argumentos. A asserção foi preservada. [Comando da suite e fila](/home/ronald/wt-codex-par-pelo-turno-111717/app/ponto/tests/par_pelo_turno_c1v13_recibo.txt:166) · [Selo vizinho](/home/ronald/wt-codex-par-pelo-turno-111717/app/relatorios/tests/test_falta_um_significado.py:177) [nao verificado]

## 27/11 14:46 · sessao 2659bd4c88e1 · E4C-F5-INSTANTE · sonda · origem=Code

## E4C-F5-INSTANTE — sonda na Sombra

**Corte (`transaction_timestamp()`)** [nao verificado]. Pela janela de 30 dias do anexo `r.md`, §4, o corte cai em **2026-11-27**, no fuso `America/Sao_Paulo`. A saída contém as contagens diárias, sem o horário do corte. `sonda.stdout:3–32` · [app/config/settings/base.py:93](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/config/settings/base.py:93)

A execução fornecida registra leitura na Sombra. Pelo contexto informado, a Sombra é o dump do dia e os números medem o código do `main` de hoje. A revisão executada e uma nova execução ficam [nao verificado]. `sonda.stdout:1`

As referências `sonda.stdout` numeram a saída raspada fornecida a partir de `sonda_leitura`. Os totais são as somas das 30 linhas. `sonda.stdout:1–32`

| dia | respostas | com_2_ou_mais_eventos_depois | sem_evento_depois | sem_celula_resolvida | anteriores_ao_horizonte | fonte |
|---|---:|---:|---:|---:|---:|---|
| 2026-10-29 | 86 | 19 | 67 | 0 | 86 | `sonda.stdout:3` |
| 2026-10-30 | 39 | 0 | 39 | 0 | 39 | `sonda.stdout:4` |
| 2026-10-31 | 44 | 4 | 40 | 0 | 44 | `sonda.stdout:5` |
| 2026-11-01 | 62 | 1 | 60 | 0 | 62 | `sonda.stdout:6` |
| 2026-11-02 | 95 | 5 | 90 | 0 | 95 | `sonda.stdout:7` |
| 2026-11-03 | 125 | 1 | 122 | 0 | 125 | `sonda.stdout:8` |
| 2026-11-04 | 34 | 1 | 32 | 0 | 34 | `sonda.stdout:9` |
| 2026-11-05 | 76 | 12 | 62 | 0 | 76 | `sonda.stdout:10` |
| 2026-11-06 | 90 | 2 | 88 | 0 | 90 | `sonda.stdout:11` |
| 2026-11-07 | 32 | 1 | 29 | 0 | 32 | `sonda.stdout:12` |
| 2026-11-08 | 45 | 19 | 26 | 0 | 45 | `sonda.stdout:13` |
| 2026-11-09 | 161 | 19 | 138 | 0 | 161 | `sonda.stdout:14` |
| 2026-11-10 | 93 | 10 | 83 | 0 | 93 | `sonda.stdout:15` |
| 2026-11-11 | 178 | 19 | 158 | 0 | 178 | `sonda.stdout:16` |
| 2026-11-12 | 163 | 26 | 136 | 1 | 163 | `sonda.stdout:17` |
| 2026-11-13 | 102 | 4 | 98 | 0 | 102 | `sonda.stdout:18` |
| 2026-11-14 | 129 | 17 | 112 | 0 | 129 | `sonda.stdout:19` |
| 2026-11-15 | 57 | 7 | 50 | 0 | 57 | `sonda.stdout:20` |
| 2026-11-16 | 142 | 13 | 129 | 0 | 142 | `sonda.stdout:21` |
| 2026-11-17 | 84 | 13 | 71 | 0 | 84 | `sonda.stdout:22` |
| 2026-11-18 | 104 | 41 | 57 | 0 | 104 | `sonda.stdout:23` |
| 2026-11-19 | 157 | 89 | 65 | 0 | 157 | `sonda.stdout:24` |
| 2026-11-20 | 133 | 133 | 0 | 0 | 77 | `sonda.stdout:25` |
| 2026-11-21 | 77 | 77 | 0 | 0 | 0 | `sonda.stdout:26` |
| 2026-11-22 | 64 | 64 | 0 | 0 | 0 | `sonda.stdout:27` |
| 2026-11-23 | 122 | 122 | 0 | 0 | 0 | `sonda.stdout:28` |
| 2026-11-24 | 123 | 123 | 0 | 0 | 0 | `sonda.stdout:29` |
| 2026-11-25 | 76 | 76 | 0 | 0 | 0 | `sonda.stdout:30` |
| 2026-11-26 | 73 | 73 | 0 | 0 | 0 | `sonda.stdout:31` |
| 2026-11-27 | 0 | 0 | 0 | 0 | 0 | `sonda.stdout:32` |
| **Total** | **2.766** | **991** | **1.752** | **1** | **2.175** | `sonda.stdout:3–32` |

_8713 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
