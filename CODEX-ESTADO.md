# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 20:21** · publicado em **17/11/2026 20:24** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 20:21 · sessao 039ca51e6204 · CURA-DE-FASE · censo · origem=Code

A classificação **DATA GARANTIDA** abaixo vale para os caminhos de produção rastreados; quando a origem de um parâmetro não foi fechada, marquei **DUVIDOSO**. As referências são relativas à raiz da cópia. [não verificado: identidade do HEAD por Git, pois a cópia não contém `.git`]

| arquivo:linha | alvo | fonte | veredito |
|---|---|---|---|
| app/api/views.py:588 | escala_vigente | `localtime(ts_efetivo).date()`, app/api/views.py:584 | DATA GARANTIDA |
| app/api/views_core.py:574 | escala_vigente | `localtime(ts_efetivo).date()`, app/api/views_core.py:570 | DATA GARANTIDA |
| app/chamados/detector_anomalias.py:146 | escala_vigente | `localtime(timestamp_batida).date()`, app/chamados/detector_anomalias.py:139–140 | DATA GARANTIDA |
| app/chamados/juizes.py:2203 | escala_vigente | Chamador passa `min(_d_perg, _d_ch)` após subtração; `_d_ch` é convertido a data, app/chamados/juizes.py:2363–2377 | DATA GARANTIDA |
| app/chamados/juizes.py:321 | escala_vigente | Parâmetro recebido de materialização: data da pergunta ou conversão do contexto, app/chamados/services/materializacao.py:613–616, :692; falta fechar a origem de todas as instâncias em memória | DUVIDOSO |
| app/chamados/juizes.py:519 | escala_vigente | `date.fromisoformat(str(data_iso)[:10])`, app/chamados/juizes.py:469–473 | DATA GARANTIDA |
| app/chamados/juizes.py:2009 | vinculo_do_dia | Chamadores encontrados omitem `data`, usando `localdate()`, app/chamados/juizes.py:1996; app/chamados/views.py:929; app/chamados/services/fio.py:509; app/chamados/services/acoes_chamado.py:422 | DATA GARANTIDA |
| app/chamados/management/commands/auditar_disputas.py:112 | escala_vigente | `timezone.localdate(b.timestamp)`, app/chamados/management/commands/auditar_disputas.py:110–111 | DATA GARANTIDA |
| app/chamados/management/commands/auditar_invariantes_chamados.py:391 | vinculo_do_dia | `c.data` de `CelulaDia` consultada pelo ORM, app/chamados/management/commands/auditar_invariantes_chamados.py:331–333; campo em app/escala/models.py:1685 | DATA GARANTIDA |
| app/chamados/management/commands/auditar_invariantes_chamados.py:1538 | vinculo_cobre_a_data | `f.data` de `FolgaDia` consultada pelo ORM, app/chamados/management/commands/auditar_invariantes_chamados.py:1527, :1536–1538; app/escala/models.py:1588 | DATA GARANTIDA |
| app/chamados/management/commands/auditar_invariantes_chamados.py:361 | q_vinculo_cobre_a_data | `p.data_evento` de pergunta consultada pelo ORM, app/chamados/management/commands/auditar_invariantes_chamados.py:347–348; `DateField`, app/chamados/models.py:2528 | DATA GARANTIDA |
| app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:87 | escala_vigente | `t.data_turno`, app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:115, :145; construída de timestamp local e `.date()`, app/ponto/turnos.py:45, :285–308, :318–323 | DATA GARANTIDA |
| app/chamados/services/carga_pergunta_viva.py:162 | escala_vigente | `dia` é parâmetro; também recebe `data_evento` de `classificar_falta`, app/escala/servico_jornada.py:280, :363. Faltou fechar todos os chamadores dessa função | DUVIDOSO |
| app/chamados/services/disputa_emissao.py:833 | escala_vigente | Chamador passa data extraída do JSON por `_ddc`, app/chamados/services/disputa_emissao.py:1246–1249; parser converte, app/chamados/data_evento.py:7–13, :27–39 | DATA GARANTIDA |
| app/chamados/services/disputa_emissao.py:923 | escala_vigente | `data_evento` é parâmetro, app/chamados/services/disputa_emissao.py:901; há caminhos com conversão de JSON, app/chamados/services/disputa_emissao.py:1184–1185, :1246–1254, mas faltou fechar todos os chamadores | DUVIDOSO |
| app/chamados/services/limbo_folgas.py:321 | escala_vigente | `dia_do_fato`, app/chamados/services/limbo_folgas.py:318; célula de `DateField` ou datas convertidas, app/escala/models.py:1685; app/chamados/juizes.py:51–54, :99–122, :162–170 | DATA GARANTIDA |
| app/chamados/services/limbo_folgas.py:200 | vinculo_cobre_a_data | `dia_do_fato`, app/chamados/services/limbo_folgas.py:197; fontes e conversões em app/chamados/juizes.py:51–54, :99–122, :162–170; app/escala/models.py:1685 | DATA GARANTIDA |
| app/colaboradores/queries.py:64 | q_vinculo_cobre_a_data | `timezone.localdate()`, app/colaboradores/queries.py:62 | DATA GARANTIDA |
| app/colaboradores/services/calendario.py:536 | vinculo_cobre_a_data | `data_ini` construída como data ou convertida do GET, app/colaboradores/services/calendario.py:204–241; janela da competência, app/ponto/janelas.py:10–21 | DATA GARANTIDA |
| app/colaboradores/services/calendario.py:537 | vinculo_cobre_a_data | `data_fim` construída como data ou convertida do GET, app/colaboradores/services/calendario.py:204–241; app/ponto/janelas.py:10–21 | DATA GARANTIDA |
| app/colaboradores/services/calendario.py:418 | vinculo_do_dia | `_data` vem da grade, app/colaboradores/services/calendario.py:363, :382–383; grade constrói dias da janela, app/escala/services/leitor_celula.py:375–390; janela em app/colaboradores/services/calendario.py:204–241 | DATA GARANTIDA |
| app/colaboradores/services/feriado_do_posto.py:103 | vinculo_do_dia | `cel.data` de consulta a `CelulaDia`, app/colaboradores/services/feriado_do_posto.py:94–96; `DateField`, app/escala/models.py:1685 | DATA GARANTIDA |
| app/colaboradores/services/ficha.py:129 | vinculo_do_dia | Chamadas produtivas identificadas usam o padrão `localtime(now()).date()`, app/colaboradores/services/ficha.py:117–118, :269–275; app/api/views_mensageria.py:1133–1134 | DATA GARANTIDA |
| app/colaboradores/services/vinculo.py:955 | vinculo_cobre_a_data | Chamador passa `timezone.localdate()`, app/colaboradores/services/estrutura.py:62 | DATA GARANTIDA |
| app/colaboradores/views.py:2652 | q_vinculo_cobre_a_data | `timezone.localdate()`, app/colaboradores/views.py:2650 | DATA GARANTIDA |
| app/core/feriados.py:161 | vinculo_do_dia | Dias construídos por aritmética de datas, app/core/feriados.py:147–150; exemplos de fontes em app/colaboradores/services/calendario.py:204–241, :347 e app/relatorios/pdf_espelho.py:298 | DATA GARANTIDA |
| app/escala/alimentacao.py:358 | vinculo_do_dia | `d` vem dos pares recebidos, app/escala/alimentacao.py:334, :350; outro carregador repassa parâmetro `data`, :320–330. Faltou fechar as fontes de todos os chamadores | DUVIDOSO |
| app/escala/alimentacao.py:285 | vinculo_cobre_a_data | `data` vem de `escala_vigente`, app/escala/servico_jornada.py:66; chamada recebe `x['virada']`, app/ponto/management/commands/censo_fase_12x36.py:413, gerada por `str(virada)`, :224 | TEXTO POSSÍVEL |
| app/escala/folgas.py:487 | vinculo_cobre_a_data | `fd.data`; chamadores identificados carregam `FolgaDia` pelo ORM, app/escala/folgas.py:535–541; app/escala/management/commands/reparentar_folgas.py:28–34; campo em app/escala/models.py:1588 | DATA GARANTIDA |
| app/escala/folgas.py:451 | q_vinculo_cobre_a_data | `data` é parâmetro, app/escala/folgas.py:442; não foi identificado chamador de produção que estabeleça sua fonte | DUVIDOSO |
| app/escala/folgas.py:539 | vinculo_cobre_a_data | `fd.data` de consulta a `FolgaDia`, app/escala/folgas.py:535–538; app/escala/models.py:1588 | DATA GARANTIDA |
| app/escala/folgas.py:624 | vinculo_cobre_a_data | `data` consultada de `CelulaDia`, app/escala/folgas.py:621–623; `DateField`, app/escala/models.py:1685 | DATA GARANTIDA |
| app/escala/management/commands/reparentar_folgas.py:31 | vinculo_cobre_a_data | `fd.data` de consulta a `FolgaDia`, app/escala/management/commands/reparentar_folgas.py:28–30; app/escala/models.py:1588 | DATA GARANTIDA |
| app/escala/models.py:929 | q_vinculo_cobre_a_data | Chamador encontrado omite `hoje`, app/escala/management/commands/vinculos_5x2_6x1_trabalha_feriado.py:15; padrão `date.today()`, app/escala/models.py:920, :926 | DATA GARANTIDA |
| app/escala/services/cadastro_realidade.py:513 | vinculo_do_dia | Dias extraídos de células consultadas pelo ORM, app/escala/services/cadastro_realidade.py:452–456, :497–498; app/escala/models.py:1685 | DATA GARANTIDA |
| app/escala/services/cadastro_realidade.py:765 | vinculo_do_dia | `hoje` é repassado por `lista`, app/escala/services/cadastro_realidade.py:748, :773, :780–781, :877; faltou fechar todas as bordas que fornecem `hoje` | DUVIDOSO |
| app/escala/services/cadastro_realidade.py:761 | q_vinculo_cobre_a_data | Mesmo parâmetro `hoje` de `lista`, app/escala/services/cadastro_realidade.py:748, :773, :780–781, :877; faltou fechar todas as bordas | DUVIDOSO |
| app/escala/services/escala_certa.py:31 | escala_vigente | Caminho identificado: motor passa `data_jornada`, app/ponto/motor_calculo_v2.py:2403–2409, :2461; vem de `.date()` ou `t.data_turno`, :884–895; app/ponto/turnos.py:45–48, :285–308 | DATA GARANTIDA |
| app/escala/services/fase_na_tela.py:161 | q_vinculo_cobre_a_data | Chamador passa `hoje=localdate()` e `contra`, app/colaboradores/views_fase.py:54, :67, :73; `contra` vem dos dias trabalhados, app/escala/services/fase_na_tela.py:40–47, :113–120; app/escala/services/propositor.py:106–118; app/ponto/turnos.py:45–48, :285–308 | DATA GARANTIDA |
| app/escala/services/propositor.py:448 | vinculo_do_dia | `d` é construído com `timedelta` a partir de `hoje`, app/escala/services/propositor.py:415–417 | DATA GARANTIDA |

Nos seis sítios de `q_vinculo_cobre_a_data`, o `Q` monta lookups `lte`/`gte` sobre os campos de vínculo, que são `DateField` ([alimentacao.py:219](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:219), [models.py:875](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:875)). **Conforme o dado fornecido**, o lookup do Django coage texto ISO válido; a coerção não foi verificada independentemente na cópia [não verificado]. A comparação Python da juíza é direta ([alimentacao.py:214](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:214)).

**LOTE 0: 40 linhas, DATA GARANTIDA 32, TEXTO POSSÍVEL 1, DUVIDOSO 7.**

## 17/11 20:20 · sessao 3e36c6765b3a · CURA-DE-FASE · censo · origem=Code

O lote tem **39 chamadas**. Nos casos duvidosos, a tabela identifica a parte da cadeia que foi localizada e o que falta comprovar; isso não demonstra, por si só, que texto alcance a chamada.

| arquivo:linha | alvo | fonte | veredito |
|---|---|---|---|
| `app/ponto/portas/regen_exportada.py:152` | `vinculo_cobre_a_data` | `_d=desde` em `app/ponto/portas/regen_exportada.py:150`; a guarda exige `date` em `:118`; chamador filtra datas em `app/chamados/services/passivo_validado.py:450` e passa o intervalo em `:453`. | DATA GARANTIDA |
| `app/ponto/precedencia.py:349` | `vinculo_do_dia` | `data` parte de `data_ini` em `app/ponto/precedencia.py:334`, parâmetro de `:259`; falta fechar as fontes dos chamadores. | DUVIDOSO |
| `app/ponto/precedencia.py:537` | `escala_vigente` | `d` deriva do parâmetro `data` em `app/ponto/precedencia.py:430,507`; um caminho converte ISO em `app/ponto/views.py:2520,2547` e passa por `app/ponto/services/ausencia.py:483,524`; demais fontes não fechadas. | DUVIDOSO |
| `app/ponto/precedencia.py:510` | `escala_vigente` | Mesma cadeia: `data` em `app/ponto/precedencia.py:430,507`; caminho convertido em `app/ponto/views.py:2520,2547` e `app/ponto/services/ausencia.py:483,524`; demais fontes não fechadas. | DUVIDOSO |
| `app/ponto/propositor.py:350` | `escala_vigente` | `dia` vem de `t.data_turno` em `app/chamados/reconciliador.py:96,113`; campo `DateField` em `app/ponto/models.py:1208,1215`. | DATA GARANTIDA |
| `app/ponto/registro_batida.py:29` | `escala_vigente` | `timezone.localtime(timestamp).date()` em `app/ponto/registro_batida.py:28`. | DATA GARANTIDA |
| `app/ponto/registro_batida.py:44` | `escala_vigente` | `timestamp` localizado e `.date()` em `app/ponto/registro_batida.py:40,43`. | DATA GARANTIDA |
| `app/ponto/services/cadastro_futuro.py:70` | `vinculo_do_dia` | `cel.data` das células consultadas em `app/ponto/services/cadastro_futuro.py:64`; `DateField` em `app/escala/models.py:1635,1685`. | DATA GARANTIDA |
| `app/ponto/services/cartorio.py:875` | `escala_vigente` | Fontes rastreadas: `cel.data` em `app/ponto/services/cartorio.py:1353,1364`, `app/ponto/services/vigia_de_hora.py:126` e `app/chamados/services/fabrica_da_celula.py:46`; ISO convertido em `app/ponto/management/commands/recusar_ponta_pequena.py:133,150`; outro caminho faz aritmética de data em `app/ponto/services/cartorio.py:378,395,445`. Campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/services/cartorio.py:1903` | `_vinculo_do_dia` | Chamadores passam `data` em `app/ponto/services/cartorio.py:1783,1819,1845`, obtida de `cel.data` em `:1353`; campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/services/cartorio.py:2035` | `escala_vigente` | `data` é parâmetro em `app/ponto/services/cartorio.py:2011`; comparação com `localdate()` em `:2016,2025` antecede a chamada, mas as fontes dos chamadores não foram fechadas. | DUVIDOSO |
| `app/ponto/services/cartorio.py:1768` | `_vinculo_do_dia` | `data=cel.data` em `app/ponto/services/cartorio.py:1353`; campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/services/cobertura.py:278` | `escala_vigente` | `dia` e `hoje` são parâmetros em `app/ponto/services/cobertura.py:247`; `hoje` pode ser fornecido e há comparação em `:273`; falta comprovar as fontes de ambos. | DUVIDOSO |
| `app/ponto/services/declarar_dia.py:58` | `escala_vigente` | `data` é parâmetro em `app/ponto/services/declarar_dia.py:56`, repassado por chamadas em `:75,109,182`; faltam as fontes de todos os caminhos. | DUVIDOSO |
| `app/ponto/services/diagnostico_escala.py:53` | `vinculo_cobre_a_data` | `d=ini` em `app/ponto/services/diagnostico_escala.py:148,150`; a API constrói `ini` de `localdate()` ou da data convertida em `app/api/views_mensageria.py:1994,2006,2009`; conversão em `:1726,1731,1759`. | DATA GARANTIDA |
| `app/ponto/services/espelho.py:286` | `vinculo_do_dia` | `d` recebe `data` em `app/ponto/services/espelho.py:675`; esta deriva de `data_ini` em `:653`; `montar_dias` recebe o parâmetro em `:365` e é chamada com `vis_ini` em `:1262`; fonte completa de `vis_ini` não fechada. | DUVIDOSO |
| `app/ponto/services/espelho.py:1065` | `vinculo_do_dia` | `fim` é parâmetro em `app/ponto/services/espelho.py:971`; há chamada em `:1227`, mas as fontes de todos os chamadores não foram fechadas. | DUVIDOSO |
| `app/ponto/services/espelho.py:724` | `vinculo_do_dia` | `data` deriva de `data_ini` em `app/ponto/services/espelho.py:653`; `montar_dias` recebe o parâmetro em `:365` e é chamada com `vis_ini` em `:1262`; fonte completa não fechada. | DUVIDOSO |
| `app/ponto/services/fechamento.py:125` | `vinculo_do_dia` | `_d=fim` em `app/ponto/services/fechamento.py:123`; chamador passa `fim.date()` em `:851`. | DATA GARANTIDA |
| `app/ponto/services/fechamento.py:191` | `vinculo_do_dia` | `_d` é chave do mapa em `app/ponto/services/fechamento.py:189`; mapa formado de `c.data` em `:605` ou pelo carregador em `app/escala/alimentacao.py:67,73`; campo em `app/escala/models.py:1685`; outros chamadores em `app/ponto/services/espelho.py:1049,1164` e `app/relatorios/pdf_espelho.py:354,419`. | DATA GARANTIDA |
| `app/ponto/services/fechamento.py:948` | `escala_vigente` | `timezone.localtime(_p_f.entrada).date()` em `app/ponto/services/fechamento.py:947`. | DATA GARANTIDA |
| `app/ponto/services/fechamento.py:229` | `vinculo_cobre_a_data` | `_d=fatia_ini` em `app/ponto/services/fechamento.py:225`; `fatia_ini` inclui parâmetro `ini` em `:132,206`. Um chamador passa `.date()` em `:754`; fontes dos outros, em `app/ponto/services/espelho.py:1164` e `app/relatorios/pdf_espelho.py:419`, não foram fechadas. | DUVIDOSO |
| `app/ponto/services/flip_auto.py:66` | `vinculo_do_dia` | `dia=timezone.localtime(ts_b).date()` em `app/ponto/services/flip_auto.py:62,63`. A chamada efetiva é `vinculo_do_dia`, embora o lote a rotule `escala_vigente` (`:66`). | DATA GARANTIDA |
| `app/ponto/services/furos_vetados.py:24` | `escala_vigente` | `data` é parâmetro em `app/ponto/services/furos_vetados.py:18`; um caminho usa `cel.data` em `:64`, outro usa `dia_do_fato` em `:101,106`; falta fechar esta última fonte. | DUVIDOSO |
| `app/ponto/services/veredito_celula.py:41` | `escala_vigente` | `data` é parâmetro em `app/ponto/services/veredito_celula.py:22`; repasses em `:211,227` e `app/ponto/services/flip_auto.py:201,215,293`; faltam as fontes de todos os chamadores. | DUVIDOSO |
| `app/ponto/services/vigia_cadastro_futuro.py:58` | `vinculo_do_dia` | `cel.data` da consulta em `app/ponto/services/vigia_cadastro_futuro.py:25,31,38,43`; campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/services/vigia_de_hora.py:47` | `escala_vigente` | `cel.data` de `CelulaDia` em `app/ponto/services/vigia_de_hora.py:117,118`; campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/services/vigia_de_hora.py:74` | `escala_vigente` | `hoje` recebe `cel.data` na chamada em `app/ponto/services/vigia_de_hora.py:130`; campo em `app/escala/models.py:1685`. | DATA GARANTIDA |
| `app/ponto/supra_juiz.py:174` | `vinculo_do_dia` | `data` é parâmetro da closure em `app/ponto/supra_juiz.py:173`; um caminho vem de `cel.data` em `app/ponto/services/cartorio.py:1353,1475`; outro lê `dia.get('data')` em `app/ponto/management/commands/emitir_furo_retroativo.py:90,110`; origem completa da grade não fechada. | DUVIDOSO |
| `app/ponto/turnos.py:2230` | `escala_vigente` | `hoje=timezone.localdate(agora)` em `app/ponto/turnos.py:2227`. | DATA GARANTIDA |
| `app/ponto/turnos.py:788` | `escala_vigente` | `data` é parâmetro em `app/ponto/turnos.py:774`; fontes dos diversos chamadores não foram fechadas. | DUVIDOSO |
| `app/ponto/turnos.py:1965` | `vinculo_do_dia` | `_d=ini` em `app/ponto/turnos.py:1963`; há aritmética prévia sobre a janela em `:1934` e `:1847`; fontes de `ini` dos chamadores em `:1920,3048` não foram fechadas. | DUVIDOSO |
| `app/ponto/turnos.py:2105` | `escala_vigente` | `data` é parâmetro em `app/ponto/turnos.py:2098`; um chamador passa `p.data_evento` em `app/chamados/reconciliador.py:1091`, campo em `app/chamados/models.py:2528`; demais fontes não fechadas. | DUVIDOSO |
| `app/ponto/turnos.py:2373` | `vinculo_do_dia` | `hoje=timezone.localdate(agora)` em `app/ponto/turnos.py:2342`. | DATA GARANTIDA |
| `app/ponto/turnos.py:3070` | `vinculo_do_dia` | `data` é parâmetro em `app/ponto/turnos.py:3065`; exemplos de chamadas em `app/escala/servico_jornada.py:363` e `app/chamados/juizes.py:322`; fontes de todos os caminhos não fechadas. | DUVIDOSO |
| `app/ponto/turnos.py:2178` | `vinculo_do_dia` | `d` parte de `ini` em `app/ponto/turnos.py:2176`; chamadores em `:2161,3057`; fonte de todos os valores de `ini` não fechada. | DUVIDOSO |
| `app/relatorios/dia_da_frota.py:42` | `escala_vigente` | API lê GET em `app/api/views_mensageria.py:1820` e chama o relatório em `:1828`; `_data_pedida` converte ISO ou constrói `date` em `:1726,1731,1748,1759,1764`. | DATA GARANTIDA |
| `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:115` | `vinculo_do_dia` | `d=ini+timedelta` em `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:109`; `ini` vem de `janela_fechamento` em `:94`, construída com `datetime.date` em `app/ponto/janelas.py:16,54`. | DATA GARANTIDA |
| `app/relatorios/pdf_espelho.py:571` | `vinculo_do_dia` | `d` é parâmetro da closure em `app/relatorios/pdf_espelho.py:565`; não foi localizada chamada da closure no arquivo, portanto a fonte de `d` não foi identificada. | DUVIDOSO |

Nenhuma linha deste lote chama `q_vinculo_cobre_a_data`; a função monta lookups `DateField` em `app/escala/alimentacao.py:219,223`. O lookup Django aceita e coage uma string ISO válida **[não verificado na cópia]**. A comparação Python relevante para a classe está em `app/escala/alimentacao.py:214`.

**LOTE 2: 39 linhas, DATA GARANTIDA 20, TEXTO POSSIVEL 0, DUVIDOSO 19.**

## 17/11 20:20 · sessao 549f10d565d5 · CURA-DE-FASE · censo · origem=Code

O sítio confirmado com texto é o da cura; esse valor também pode alcançar as duas chamadas internas de `escala_vigente` quando há vínculos candidatos (`app/ponto/management/commands/censo_fase_12x36.py:224`, `app/ponto/management/commands/censo_fase_12x36.py:413`, `app/escala/servico_jornada.py:63`, `app/escala/servico_jornada.py:66`).

Para encurtar as referências na tabela, **C** significa `app/ponto/management/commands/`. Referências `:linha` na coluna fonte pertencem ao arquivo da própria linha.

| arquivo:linha | alvo | fonte | veredito |
|---|---|---|---|
| app/escala/services/semana_do_posto.py:75 | q_vinculo_cobre_a_data | `seg` vem de `limites_da_semana(inicio)` e recebe `timedelta` antes da chamada (:51, :55, :75, :132, :134). | DATA GARANTIDA |
| app/escala/servico_jornada.py:66 | vinculo_do_dia | Parâmetro `data` (:15); `_curar` passa `x['virada']`, montada com `str(virada)` (C`censo_fase_12x36.py:224`, :413). | TEXTO POSSIVEL |
| app/escala/servico_jornada.py:544 | escala_vigente | Parâmetro `data_evento` de `procedencia_fina` (:513); origem de todos os chamadores não estabelecida [não verificado]. | DUVIDOSO |
| app/escala/servico_jornada.py:63 | vinculo_do_dia | Parâmetro `data` (:15); `_curar` passa `x['virada']`, montada com `str(virada)` (C`censo_fase_12x36.py:224`, :413); este ramo depende de célula geradora. | TEXTO POSSIVEL |
| app/escala/servico_jornada.py:233 | escala_vigente | `data_evento` é parâmetro (:220); um chamador repassa o parâmetro de `classificar_falta` (:280, :364), cuja origem completa falta [não verificado]. | DUVIDOSO |
| app/escala/servico_jornada.py:362 | escala_vigente | `data_evento` é parâmetro (:280); `motivo_da_lei` repassa seu próprio `data` (:558, :566); fontes completas faltam [não verificado]. | DUVIDOSO |
| app/escala/utils.py:2194 | vinculo_do_dia | `data` é parâmetro (:2142), repassado por `intervalo_declarado_do_dia` e `minutos_previstos_do_dia` (:2197, :2212, :2230, :2260); nem todas as fontes foram estabelecidas [não verificado]. | DUVIDOSO |
| app/escala/utils.py:988 | vinculo_do_dia | `d` vem de `ref - timedelta` (:979, :987, :989, :992); `ref` ausente usa `localdate`. | DATA GARANTIDA |
| app/escala/utils.py:1311 | vinculo_do_dia | `d` vem de `ref - timedelta` (:1290, :1306, :1313, :1316); o default de `ref` usa `.date()`. | DATA GARANTIDA |
| app/folha/export.py:1255 | vinculo_cobre_a_data | `dia` ← `d` ← parâmetro `ini` de `_dias_sem_decisao` (:1231, :1254, :1264, :1267); fontes completas de `ini` não estabelecidas [não verificado]. | DUVIDOSO |
| app/folha/previa.py:81 | vinculo_cobre_a_data | `d = cel.data` (:78); `CelulaDia.data` é `DateField` (app/escala/models.py:1635, :1685). | DATA GARANTIDA |
| app/ponto/calculador/alimentacao.py:196 | vinculo_do_dia | `_dia` vem dos valores de `resultado.dia_da_jornada` (:193, :195); o motor mescla mapas (app/ponto/motor_calculo_v2.py:1073), mas todas as origens e todos os resultados aceitos não foram fechados [não verificado]. | DUVIDOSO |
| app/ponto/calculador/alimentacao.py:432 | vinculo_do_dia | `_dia` ← `k` (:492); `k` converte strings com `date.fromisoformat` e os demais valores passam pela comparação com os limites (:473, :474). A origem dos valores não string como datas é [não verificado]. | DUVIDOSO |
| C`aplicar_col900.py:229` | vinculo_do_dia | `dia = date.fromisoformat(it['dia'])` (:226). | DATA GARANTIDA |
| C`aplicar_col900.py:176` | vinculo_do_dia | `dia` vem de `dias`, cujos itens são convertidos com `date.fromisoformat` (:159, :172). | DATA GARANTIDA |
| C`ausencias_sem_efeito_na_celula.py:55` | q_vinculo_cobre_a_data | `d` é chave de `cobertura_ausencia_periodo` (:44, :48); a cobertura constrói as chaves por aritmética de datas (app/ponto/turnos.py:2777, :2790, :2794). | DATA GARANTIDA |
| C`censo_col900.py:111` | vinculo_do_dia | `dia` é chave construída de `CelulaDia.data` (:104, :110; app/escala/models.py:1685). | DATA GARANTIDA |
| C`censo_fase_12x36.py:413` | escala_vigente | `x['virada']` é montada como `str(virada)` (:224), repassada sem conversão (:413). | TEXTO POSSIVEL |
| C`censo_fase_12x36.py:161` | vinculo_do_dia | `_d` vem de `dias`, construída por `ini + timedelta` e `(fim - ini).days` (:129, :134, :160). | DATA GARANTIDA |
| C`curar_residuo_bug115.py:85` | escala_vigente | `d` vem de `dias_do_fato` (:77, :80); os ramos incluem data da célula, JSON convertido com `fromisoformat` e `localdate` (app/chamados/juizes.py:188, :194, :204); demais ramos [não verificado]. | DUVIDOSO |
| C`detectar_ausencias.py:140` | escala_vigente | `data_ref` vem de `hoje` ou `ontem` (:137, :139); `hoje = agora.date()` (:67, :71, :72). | DATA GARANTIDA |
| C`diff_calculador.py:387` | vinculo_do_dia | `_dia` ← `k` na chamada (:479); `k = date.fromisoformat(k_str)` (:428, :429). | DATA GARANTIDA |
| C`diff_col900.py:69` | vinculo_do_dia | `dia = date.fromisoformat(it['dia'])` (:66). | DATA GARANTIDA |
| C`dna_x_batida_real.py:77` | vinculo_do_dia | `d` é chave de `cels`, construída com `CelulaDia.data` (:64, :76; app/escala/models.py:1685). | DATA GARANTIDA |
| C`emitir_furo_retroativo.py:130` | escala_vigente | `data` vem de `grade_da_celula` (:60, :90, :91); o leitor usa `_cur = fim` como `data` e decrementa um dia (app/escala/services/leitor_celula.py:376, :390, :444); `fim` é convertido com `fromisoformat` (:41). | DATA GARANTIDA |
| C`emitir_turno_aberto_retroativo.py:104` | escala_vigente | `data` vem de `t.data_turno` (:88, :89), produzido pelo pareamento (app/ponto/turnos.py:318); `_data_do_turno` declara retorno `date` (app/ponto/turnos.py:248). | DATA GARANTIDA |
| C`gerar_celulas.py:287` | vinculo_cobre_a_data | `d` é parâmetro de `_outro_vinculo_cobre` (:269), chamado com o `d` da regeneração (app/ponto/portas/celula.py:227, :254); origem de `desde` para todos os chamadores falta [não verificado]. | DUVIDOSO |
| C`processar_alertas_avancados.py:37` | escala_vigente | `ontem` deriva de `agora.date()` (:20, :21, :22). | DATA GARANTIDA |
| C`processar_alertas_turno.py:134` | escala_vigente | `tm` vem de `TurnoMaterializado.objects` (:76, :81); `data_turno` é `DateField` (app/ponto/models.py:1208, :1215). | DATA GARANTIDA |
| C`processar_alertas_turno.py:106` | escala_vigente | Mesmo `tm` do queryset (:76, :81); `TurnoMaterializado.data_turno` é `DateField` (app/ponto/models.py:1215). | DATA GARANTIDA |
| C`reconciliar_grade.py:157` | escala_vigente | `dt = self._dt(..., ref)` (:133); `_dt` retorna `datetime.date(...)` ou `ref - timedelta` (:349, :356, :359); `ref` vem de `fromisoformat` ou `.date()` (:78). | DATA GARANTIDA |
| C`reconciliar_grade.py:196` | vinculo_do_dia | `_d_rg = win0` e avança por `timedelta` (:194, :197); `win0` deriva de `ref` convertido ou `.date()` (:78, :79). | DATA GARANTIDA |
| C`rejulgar_ausencia_em_folga.py:65` | escala_vigente | `d` é chave de `folgas_lancadas_do_periodo` (:64); construída de `FolgaDia.data` (app/ponto/precedencia.py:204, :208; app/escala/models.py:1582, :1588). | DATA GARANTIDA |
| C`tripwire_celulas.py:107` | vinculo_do_dia | `cel.data` é `CelulaDia.data`, `DateField` (app/escala/models.py:1635, :1685). | DATA GARANTIDA |
| C`vigia_plantio_overnight.py:42` | escala_vigente | `t` vem de `turnos_do_colab` (:38); o pareador atribui `data_turno` por `_data_do_turno` (app/ponto/turnos.py:318), cujo retorno declarado é `date` (app/ponto/turnos.py:248). | DATA GARANTIDA |
| app/ponto/nucleo.py:26 | vinculo_cobre_a_data | `d` é parâmetro (:18); `dt_fim_previsto_de` repassa `data_turno` (:31, :36), inclusive a partir de turno (:55); todas as origens desse parâmetro não foram fechadas [não verificado]. | DUVIDOSO |
| app/ponto/portas/celula.py:229 | vinculo_cobre_a_data | `d = desde` (:227); `desde` é parâmetro da porta (:112), sem conversão ali; todas as fontes faltam [não verificado]. | DUVIDOSO |
| app/ponto/portas/celula.py:906 | vinculo_cobre_a_data | `d` vem de `dias`, construído com `ini + timedelta` e `(fim - ini).days` (:886, :901). | DATA GARANTIDA |
| app/ponto/portas/celula.py:339 | vinculo_cobre_a_data | `_d` vem de `rejulgar` (:337), mas só alcança a chamada se pertencer ao conjunto de `CelulaDia.data` consultado no banco (:335, :336, :338; app/escala/models.py:1685). | DATA GARANTIDA |
| app/ponto/portas/celula.py:909 | vinculo_do_dia | Mesmo `d` da lista construída por aritmética de datas (:886, :901). | DATA GARANTIDA |

Sobre os dois sítios `q_vinculo_cobre_a_data`: o `Q` monta lookups de `DateField` (`app/escala/alimentacao.py:219`, :223, :224; `app/escala/models.py:875`, :876). O Django normalmente coage uma string ISO válida nesses lookups; a implementação do Django nesta cópia não foi verificada, portanto essa afirmação é **[não verificado]**. Já a juíza Python compara diretamente `ec.data_inicio <= data` (`app/escala/alimentacao.py:214`).

**LOTE 1: 40 linhas, DATA GARANTIDA 26, TEXTO POSSIVEL 3, DUVIDOSO 11.**

## 17/11 20:20 · sessao dc5b573b873d · - · - · origem=Ronald (tmux)

Rastreamento das linhas 28 a 40 enviado ao agente principal, com as cadeias de data e os pontos que permanecem duvidosos.

## 17/11 20:19 · sessao 286e59cd4048 · - · - · origem=Ronald (tmux)

Resultados dos 13 sítios enviados ao agente principal, com as cadeias verificadas e os pontos que permanecem duvidosos.

## 17/11 20:19 · sessao f878465dd084 · - · - · origem=Ronald (tmux)

Investigação das 13 chamadas atribuídas concluída e enviada ao agente principal, com as cadeias verificadas e as fontes que permanecem duvidosas. [não verificado]

## 17/11 20:19 · sessao af38b8160005 · - · - · origem=Ronald (tmux)

Resultados e referências das linhas 15 a 27 foram enviados ao agente principal.

## 17/11 20:19 · sessao 9bbd47c9c14d · - · - · origem=Ronald (tmux)

Rastreamento enviado ao agente principal, com fontes, vereditos e as incertezas identificadas.

## 17/11 20:19 · sessao 89947f98b6af · - · - · origem=Ronald (tmux)

Resultado dos 13 sítios enviado ao agente principal, com as cadeias verificadas e as lacunas identificadas.

## 17/11 20:18 · sessao 623b984d2c9f · PORTA-DO-EXPORT-SEM-BECO · verificador · origem=Code

**PROCEDE: NÃO.** O selo ainda permite que o ESTADO publique um número diferente do devolvido pelo leitor único: ele compara a saída do ESTADO com a frase produzida pelo mesmo medidor, e só testa a frase da porta do export com o valor sintético `2`. [bin/tests/test_recusa_estado.sh:81](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:81), [bin/tests/test_recusa_estado.sh:93](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:93)

1. **BLOQUEIA 1 continua aberto.** Um mutante concreto é inserir, em `linha_porta_export`, depois do retorno para `sem_arquivo` e antes do retorno que formata a frase: [bin/recusa_estado.py:71](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:71)

   ```python
   if d['recusas'] != 2:
       d = dict(d, recusas=d['recusas'] + 1)
   ```

   Para qualquer contagem diferente de `2`, a frase passa a mostrar uma unidade a mais que `porta_export_falhou_hoje` devolveu. A AST nova examina os retornos de `ler` e `ler_porta_export` e proíbe certos nomes, mas não verifica essa alteração em `linha_porta_export`; o caso sintético com `2` permanece igual e a comparação do ESTADO usa a mesma função alterada dos dois lados. [bin/recusa_estado.py:68](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:68), [bin/tests/test_recusa_estado.sh:37](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:37), [bin/tests/test_recusa_estado.sh:83](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:83), [bin/tests/test_recusa_estado.sh:90](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:90)

   A passagem é uma **conclusão estática**, condicionada às demais etapas do selo passarem e às duas leituras da raiz permanecerem coerentes; não executei o mutante nem o selo, pois o próprio selo cria e remove arquivos temporários. Resultado executado: **[não verificado]**. [bin/tests/test_recusa_estado.sh:59](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:59)

2. **BLOQUEIA 2: há registro de vermelho, mas não prova completa das mutações.** O anexo fornecido registra, para M1–M5, nome, arquivo, `diff stat`, mensagem e `rc=1`; isso informa o motivo *reportado* pelo selo para cada execução. Porém, o `diff stat` não mostra as linhas alteradas: não comprova que cada mutação corresponde ao nome atribuído, nem permite avaliar se houve outra alteração com o mesmo tamanho. A autenticidade e a reprodução do log são **[não verificado]**. [anexo `instr_v5_mutantes.log`, fornecido na pergunta: não verificado]

3. **Quem escreve e quem lê os valores:**

   | Valor ou sinal | Quem escreve ou produz | Quem lê |
   |---|---|---|
   | Linhas sintéticas do teste | O próprio selo cria o `ponto.log` temporário e escreve as portas e datas de teste. [selo:70](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:70) | `ler` e `ler_porta_export`; o selo verifica ausência, contagens, esperado e frase. [selo:68](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:68), [selo:81](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:81) |
   | Recusas do Resolver dia | Os envelopes chamam `registrar`, que emite a linha no logger; a configuração SaaS encaminha `ponto` ao arquivo. [respostas.py:54](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/respostas.py:54), [respostas.py:78](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/respostas.py:78), [recusa_critica.py:70](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:70), [saas.py:160](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/config/settings/saas.py:160), [saas.py:181](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/config/settings/saas.py:181) | `contar` lê o log e produz `recusas` e `por_usuario`; `resolver_dia_recusado` acrescenta o rótulo; o medidor e o ESTADO consomem o resultado. [recusa_critica.py:95](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:95), [recusa_critica.py:108](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:108), [recusa_critica.py:126](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:126), [gerar_estado.py:265](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:265) |
   | Falhas ou divergências da porta do export | `_recusa` chama o mesmo `registrar` com a porta do export. [porta_export.py:1063](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/folha/porta_export.py:1063) | `porta_export_falhou_hoje` conta as linhas e define `esperado = 0`; o medidor formata contagem, esperado e vermelho, e o ESTADO usa a frase. [recusa_critica.py:133](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/app/core/recusa_critica.py:133), [recusa_estado.py:60](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:60), [gerar_estado.py:274](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:274) |
   | `sem_arquivo`, data e frases do ESTADO | Os dois métodos do medidor produzem `sem_arquivo` quando falta o arquivo; o gerador fornece uma data e escreve o ESTADO. [recusa_estado.py:37](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:37), [recusa_estado.py:60](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/recusa_estado.py:60), [gerar_estado.py:270](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:270), [gerar_estado.py:399](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:399) | O selo lê os retornos sintéticos e compara as duas frases do medidor com linhas do ESTADO gerado. [selo:83](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:83), [selo:100](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:100) |

4. **FILAs 4–7:** permanecem como riscos já apontados: duas leituras em momentos distintos; erro do gerador ocultado; igualdade dos fusos sustentada no código por comentário; e AST que procura substrings sem impedir reatribuição de `_hoje_recusa`. [selo:90](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:90), [selo:93](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:93), [gerar_estado.py:267](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/gerar_estado.py:267), [selo:30](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:30)

   A mudança não demonstra, por si, que qualquer uma dessas quatro filas tenha virado um **novo** bloqueio. A falha de integridade demonstrada acima mantém aberto o BLOQUEIA 1; a checagem de `: 2**` reduz o caso da antiga FILA 3 apenas para esse exemplo sintético. [selo:83](/home/ronald/codex_ws/6638cb39e21e625829c4b2ad1c5c4565c50f18e7/bin/tests/test_recusa_estado.sh:83)

## 17/11 20:18 · sessao 7e1d803ea754 · E4C-F1-B2 · verificador · origem=Code

A tabela ainda precisa de correção: há **7 linhas procedentes entre os 26 sítios pedidos**, além de 6 itens comuns ou seções procedentes. O descarte de B4 resolve a colisão física, mas deixa duas contradições; os descartes de B9a e B9b não demonstram compatibilidade com o aval posterior.

Os caminhos de código abaixo são relativos a `app/chamados/`, salvo indicação diferente. Os avais são os anexos transcritos na solicitação, identificados por item.

| ID | Revisão |
|---|---|
| Cabeçalho | **PROCEDE** — O hash integral da cópia não pôde ser confirmado por Git: a cópia não contém repositório Git [não verificado]. Além disso, o cabeçalho declara zero indecisos enquanto deixa P21 no topo; o aval já manda usar a mesma leitura nos comandos que escrevem por item (anexo 18:45, item 2; `management/commands/reabrir_orfas_enterradas.py:85`, `:157`). |
| F0 | **CONFERE** — A guarda retorna fora da cerca; há quatro chamadas abridoras no código de produção (`services/fio.py:1396-1398`; `reconciliador.py:553`, `:1701`; `services/becos.py:66`; `services/disputa_emissao.py:1909`). |
| F1 | **CONFERE** — Cerca aninhada executa no contexto externo, sem abrir savepoint próprio (`services/fio.py:1433-1434`). |
| F2 | **CONFERE** — O abridor testa a anotação de veto dentro do atomic e devolve a recusa após o rollback (`services/fio.py:1439-1446`). |
| F3 | **CONFERE** — A guarda antecede o save de pergunta existente e o update em lote (`models.py:3574-3577`, `:3400-3407`). |
| F4 | **PROCEDE** — `evento` protege sua própria gravação, mas a consulta da última retenção ocorre antes dele e fora desse tratamento; falha nessa leitura pode subir, contrariando a garantia proposta de que falha da trilha não derruba o ato (`reconciliador.py:489-493`; `app/core/observ.py:94-105`, `:133-136`; anexo 18:45, item 6). |
| F5 | **CONFERE** — O juiz existente retorna `None` para dia indecidível e usa memo; o resumo e a lista são exigências do aval, ainda não uma implementação demonstrada (`services/fio.py:1358-1368`; anexo 17:20, R3). |
| F6 | **CONFERE** — A extração pode preservar o universo se mantiver exatamente dia do turno, dia **gravado**, exclusão de conversa e o `continue` para conjunto vazio; o juiz e a agregação permanecem separados (`services/fio.py:1399-1422`). |
| F7 | **CONFERE** — A regra da validação existe no caminho comum; o retorno antecipado da folga fica como achado (`services/validacao.py:711-712`, `:740-742`; anexo 18:45, item 2(i)). |
| F8 | **PROCEDE** — A lista descrita como deduplicada por `(pk, motivo)` não especifica como preservar **duas entidades de trilha** para o mesmo pk exigidas por B2a/B2b; as chamadas são sequenciais e `ch_novo` aparece depois (`reconciliador.py:977-1001`, `:1664-1666`). O coletor proposto ainda não existe; `_ESPERAS_163` é somente exemplo de contexto (`services/materializacao.py:1726-1734`). |
| F9 | **CONFERE** — O ponto proposto fica depois dos dois retornos sem escrita e antes da lavra e do save (`models.py:3241-3242`, `:3264-3273`). |
| INDECISOS | **PROCEDE** — A9d está decidido, mas P21 não deve permanecer em “manter ou trocar”: a regra posterior inclui comando de sistema e exige a mesma leitura de dias (`management/commands/reabrir_orfas_enterradas.py:85`, `:94-99`, `:157`; `services/fio.py:1403-1413`; anexo 18:45, item 2(ii)). |
| ACHADOS | **CONFERE** — As seis portas ficam como lista sem obra e sem cerca por decisão expressa; a folga passa antes da regra comum (`services/validacao.py:711-712`, `:740-742`; anexo 19:50, item 3). |
| RED-NOVO-1 | **CONFERE** — O gate chama o reaponte e hoje levanta `ValueError` quando ele recusa; o caso adicional precisa distinguir recusa por veto (`models.py:435-439`, `:689`). |
| RED-NOVO-2 | **CONFERE** — O controle deve chegar a `carimbar` e sair sem escrita para testar a posição do julgamento (`services/materializacao.py:2161-2167`; `models.py:3241-3242`, `:3264-3266`). |
| RED-NOVO-3 | **PROCEDE** — O controle especificado como **não ENTREGUE** não demonstra que `julga_antes_de_carimbar` contaria indevidamente uma saída sem escrita: para morder esse mutante, o controle precisa ser vetável e sair por C6 ou fora do fato (`models.py:3241-3242`, `:3264-3266`; `services/fio.py:1418-1419`). |
| DECISÕES | **PROCEDE** — A decisão B4 repete o descarte apesar das contradições de alvo e de dia; P21 também não deve ficar pendente da escolha manter/trocar (`services/fio.py:1403-1411`, `:969-976`; `management/commands/reabrir_orfas_enterradas.py:85`, `:157`; anexo 18:45, item 2(ii)). |
| A1 | **CONFERE** — O save de justificativa e as escritas de pergunta ficam no ato cercado; o resolver aninhado não abre outra unidade (`reconciliador.py:543-580`; `services/fio.py:1433-1446`). |
| A2 | **CONFERE** — Resolver e fecho ficam sob o abridor do beco; o veto aninhado pertence ao abridor externo (`services/becos.py:42-72`; `services/fio.py:1433-1446`). |
| A3 | **CONFERE** — Direto, resolver abre cerca; aninhado, executa na existente (`services/disputa_emissao.py:1902-1914`; `services/fio.py:1433-1434`). |
| A4 | **CONFERE** — A transição alcança o gate de reata e a relavra; fora de cerca o tratamento deve ser por item (`models.py:435-439`, `:642-656`, `:689`, `:307-312`; anexo 18:45, itens 2-3). |
| A5 | **CONFERE** — O laço da relavra chama a escrita por pergunta; dentro da cerca prevalece o veto do abridor (`models.py:307-312`; `services/fio.py:1439-1446`; anexo 18:45, item 3). |
| A6 | **CONFERE** — Encerrar passa pelo gate e pela gravação do estado; os pontos de pergunta são os identificados na tabela (`models.py:1686-1693`, `:435-439`, `:307-312`). |
| A7 | **CONFERE** — O caminho da tranca passa pelo gate e pela relavra, sem abridor próprio (`models.py:1711`, `:307-312`; `services/competencia_trancada.py:207-209`; anexo 18:45, item 3). |
| A8 | **CONFERE** — Filtrar depois do universo e antes de `alvo` também exclui a vetada das decisões, células e disputas derivadas (`models.py:1601-1609`, `:1621`, `:1638-1649`). |
| A9a | **CONFERE** — O fecho por desligamento continua sem cerca; a cadeia S144 posterior é ato de sistema por item (`models.py:2009-2012`; `signals.py:164-167`; anexo 18:45, itens 1-4). |
| A9c | **CONFERE** — Os `continue` do S144 e as duas saídas de `carimbar` antecedem F9 (`services/materializacao.py:2154-2167`; `models.py:3241-3242`, `:3264-3273`). |
| A9d | **CONFERE** — O aval posterior decide disputa fechada e pergunta vetada sem gravação; F9 permite retorno sem exceção (`services/competencia_trancada.py:222-229`; `signals.py:164-167`; `models.py:3264-3273`; anexo 19:50, item 2). |
| B1a | **CONFERE** — `False` sai pelo `continue`, fora de `n`; a trilha sem chamado deve ter a pergunta como alvo (`reconciliador.py:628-645`; anexo 17:50, item 1). |
| B1b | **CONFERE** — O fecho posterior pode disparar S144; seu julgamento fica em F9 (`reconciliador.py:652-660`; `signals.py:164-167`; `models.py:3264-3273`). |
| B2a | **PROCEDE** — A saída sem fecho confere, mas a especificação de duas trilhas para um pk não fecha com o coletor F8 descrito apenas como lista deduplicada (`reconciliador.py:400-415`, `:1664-1666`, `:977-1001`). |
| B2b | **PROCEDE** — `ch_novo` é o chamado da costura e a recusa retorna `False`, mas falta preservar esse segundo alvo sem duplicar a contagem já feita em B2a (`reconciliador.py:977-1001`, `:1664-1666`). |
| B2c | **CONFERE** — Costura e resolver estão no mesmo abridor HIST; a recusa devolve `False` (`reconciliador.py:1690-1705`; `services/fio.py:1433-1446`). |
| B2d | **PROCEDE** — `(False, mot)` é o retorno de `reapontar_resolvedora`; o chamador de B2d o consome e devolve apenas `False`, não o par afirmado na tabela (`reconciliador.py:1712-1716`). |
| B3a | **CONFERE** — `encerrar_fantasma` delega ao carimbo; `False` pula o evento (`models.py:3421-3429`; `reconciliador.py:1191-1193`). |
| B3b | **CONFERE** — O fecho alcança S144 e, nele, F9 depois das saídas sem escrita (`reconciliador.py:1216-1218`; `services/materializacao.py:2154-2167`; `models.py:3264-3273`). |
| B4 | **PROCEDE** — O risco de índice ao datar a principal com perdedora viva está documentado, mas a célula manda trilha ora ao chamado destino, ora sempre à pergunta. Também afirma que a porta lê `data_evento` da instância: ela lê o **gravado** e o dia do turno (`services/fio.py:939-947`, `:969-976`, `:1403-1411`; `reconciliador.py:489-493`). |
| B5 | **CONFERE** — `lavrar` grava por item; o backfill citado usa o escritor em lote, não essa chamada (`veredito_lavrado.py:240-250`, `:254-275`; `management/commands/backfill_veredito_lavrado.py:143-146`). |
| B6 | **CONFERE** — O `bulk_update` usa a lista recebida e não passa pelo `save` de cada pergunta (`veredito_lavrado.py:240-251`; `models.py:3565-3577`). |
| B7 | **CONFERE** — O ponto antes do save fica depois dos dois `continue` do laço (`services/relavra_via.py:117-127`). |
| B8 | **PROCEDE** — O pré-veto existente usa dia gravado e `novo_dia` e devolve recusa antes da porta; a tabela não demonstra a mesma leitura F6 nem o contrato por item que conta e deixa os demais seguirem, inclusive sob cerca (`services/fio.py:1105-1123`, `:1403-1419`; `models.py:3072-3076`; anexo 18:45, item 2(ii)). |
| B9a | **PROCEDE** — O código confirma o aval anterior por grupo, mas o aval posterior inclui backfill e determina julgamento por item; o descarte não demonstra que a regra antiga prevalece nessa colisão (`management/commands/backfill_decisoes_pergunta.py:244-262`; anexo 18:45, item 2(ii)). |
| B9b | **PROCEDE** — Grupo na ida e reversão tudo ou nada existem, mas sua prevalência sobre o comando posterior por item não foi resolvida pelo descarte (`management/commands/migrar_data_evento_dia_do_turno.py:380-393`, `:476-495`; anexo 18:45, item 2(ii)). |

**Quem escreve e quem lê:** `recusa_motivo` é escrito, entre outros pontos, pelo materializador e pela recusa humana, e lido pelo próprio materializador e pela validação (`services/materializacao.py:1676-1690`; `services/validacao.py:262-266`, `:916-922`). Os carimbos `materializada_em`, `materializada_por` e `via_resolucao` são atribuídos pela porta de carimbo e lidos pelo C6 (`models.py:3218-3222`, `:3260-3273`; `juizes.py:1389-1405`). `evento` cria `LogAuditoria`; `registrar_retencao` lê a última linha para deduplicar (`app/core/observ.py:94-105`, `:133-136`; `reconciliador.py:489-493`). `_CERCA` é escrito e restaurado pelo abridor e lido pela guarda (`services/fio.py:1381`, `:1396`, `:1433-1436`, `:1449`). O coletor F8 é proposto, portanto seus escritores e leitores ainda não existem na cópia; `_ESPERAS_163` apenas demonstra a técnica de `ContextVar` (`services/materializacao.py:1726-1734`).

A extração F6 deve preservar a consulta ao **dia gravado**, inclusive quando B4 já atribuiu outro dia em memória; tratar essa atribuição como leitura existente mudaria o predicado. A ativação exclusiva do coletor por sistema e sua ausência em portas humanas são requisitos do desenho, ainda não comportamento implementado (`services/fio.py:917-928`, `:1403-1413`; `services/materializacao.py:1726-1734`).

**PROCEDE: 13** — 7 dos sítios e 6 itens comuns/seções. **LINHAS RESPONDIDAS: 26 de 26.**

## 17/11 20:17 · sessao 015814b2b862 · - · - · origem=Ronald (tmux)

- **B2d — PROCEDE:** `reapontar_resolvedora` devolve `(False, motivo)`, mas `reatar_pergunta_orfa` trata o par e devolve apenas `False` ao seu chamador ([reconciliador.py:1712](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:1712)).

- **B2a/B2b e F8 — PROCEDE:** a tabela descreve um coletor de `(pk, motivo)` deduplicado por pk, mas exige duas trilhas com alvos diferentes para a mesma pergunta. As chamadas são sequenciais, e o chamado novo só aparece no segundo caminho; o desenho precisa preservar os dois alvos sem duplicar a contagem ([reconciliador.py:1664](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:1664), [reconciliador.py:998](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:998)).

- **P21/INDECISOS — PROCEDE:** o aval transcrito exige a mesma leitura de dias para comando de sistema e nenhum segundo leitor; a tabela deixa indeciso se mantém o leitor próprio de P21 ([reabrir_orfas_enterradas.py:85](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/reabrir_orfas_enterradas.py:85), [fio.py:1403](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1403)).

- **A9d/F9:** as duas saídas sem escrita precedem a lavra e o save, como descrito; S144 chama `carimbar` depois dos dois `continue` ([models.py:3241](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3241), [models.py:3264](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3264), [materializacao.py:2154](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/materializacao.py:2154)).

## 17/11 20:17 · sessao 3ae0314e0af1 · E4C-F1-B2 · verificador · origem=Code

| id | Revisão |
|---|---|
| P1 | **PROCEDE** — O ramo de dia diferente mantém `ReusoSemRechave`, que desfaz o ato, enquanto o aval vigente determina recusa por item e continuação dos demais. O ramo sem `upd` não escreve. `models.py:3060`, `models.py:3072`, `models.py:3117`. |
| P3 | **CONFERE** — O julgamento precede a decisão e o carimbo; a retirada do motivo e a tentativa de fecho continuam. `models.py:2145`, `models.py:2147`, `models.py:2163`. |
| P4 | **CONFERE** — Os alvos já excluem as vias de saída sem escrita da porta; o segundo laço decide o fecho. `encerrar_perguntas_com_fato.py:31`, `encerrar_perguntas_com_fato.py:65`, `models.py:3415`. |
| P5 | **CONFERE** — A seleção já restringe à via da competência; a recusa por item precede o descarimbo. `competencia_trancada.py:276`, `models.py:3459`. |
| P6 | **CONFERE** — A validação comum tem regra própria; o retorno antecipado da folga permanece no achado X4. `validacao.py:711`, `validacao.py:740`. |
| P7 | **CONFERE** — A devolução está após a segunda verificação de competência da validação humana. `validacao.py:777`, `validacao.py:781`. |
| P11 | **CONFERE** — Há escrita com `_pend` mesmo se `ts` for `None`; sem `_pend`, essa saída não conta por P11. `materializacao.py:1347`, `materializacao.py:1353`. |
| P12 | **CONFERE** — O ponto é o carimbo de identidade efetivo do ramo; a chamada anterior de recusa tem julgamento próprio em P13. `materializacao.py:1450`, `materializacao.py:1461`. |
| P13 | **CONFERE** — Julgar apenas com `_carimba or _muda_motivo` evita contar a chamada que não escreve nem anota. `materializacao.py:1676`, `materializacao.py:1680`, `materializacao.py:1691`. |
| P14 | **CONFERE** — O `save` ocorre somente no ramo `_muda_motivo`; a chamada isolada não conta. `materializacao.py:1688`, `materializacao.py:1690`. |
| P15 | **PROCEDE** — A saída C6/fora do fato não conta **por P15**, mas a tabela afirma que não conta no ato: a reprovação acrescenta erro e o `finally` pode tentar outra escrita em P13; se essa escrita for vetada, ela conta. `models.py:3241`, `models.py:3264`, `materializacao.py:1968`, `materializacao.py:1976`, `materializacao.py:1990`. |
| P17 | **CONFERE** — A linha fora do fato permanece na captura para acordar célula, mas é excluída da decisão e do update; não conta como veto da escrita de pergunta. `disputa_emissao.py:1625`, `disputa_emissao.py:1645`, `disputa_emissao.py:1648`, `models.py:3405`. |
| P18 | **CONFERE** — Resposta automática é ato de sistema; texto presente sai sem escrita. A validação comum barra, mas a folga retorna antes da guarda. `disputa_emissao.py:1663`, `disputa_emissao.py:1677`, `validacao.py:711`, `validacao.py:740`. |
| P19 | **CONFERE** — O ponto proposto vem após o retorno sem escrita e antes da atribuição e do save. `disputa_emissao.py:1692`, `disputa_emissao.py:1696`, `disputa_emissao.py:1698`. |
| P20 | **CONFERE** — O laço relê, passa pelos `continue` e só então tenta descarimbar; as vias de identidade ficam fora da seleção. `relavra_via.py:181`, `relavra_via.py:236`, `relavra_via.py:242`, `relavra_via.py:246`. |
| P21 | **PROCEDE** — O comando usa dia gravado em leitor próprio; a porta usa turno e dia gravado. A tabela corretamente mantém a decisão sobre o leitor existente no topo. `reabrir_orfas_enterradas.py:86`, `reabrir_orfas_enterradas.py:94`, `fio.py:1403`, `fio.py:1409`. |
| X1 | **CONFERE** — Os `continue` e as duas saídas sem escrita de `carimbar` precedem F9; no caminho da tranca, a disputa fecha e a pergunta vetada conta. `materializacao.py:2161`, `materializacao.py:2167`, `models.py:3241`, `models.py:3264`, `competencia_trancada.py:229`. |
| X2 | **CONFERE** — O K6 existente não substitui a leitura comum; o ponto proposto fica após `campos` vazio e antes do save. `validacao.py:338`, `validacao.py:346`, `validacao.py:351`, `fio.py:1409`. |
| X3 | **CONFERE** — O julgamento após o impedimento e antes do primeiro `registrar` evita decisão sem o update correspondente. `validacao.py:432`, `validacao.py:439`, `validacao.py:440`. |
| X5 | **CONFERE** — O laço é de sistema e pode seguir por item; o laço posterior de chamados permanece. `veredito_celula.py:189`, `veredito_celula.py:202`. |
| X6 | **CONFERE** — A saída idempotente precede o carimbo. O descarte B9a tem fundamento no aval por grupo registrado no código. `fio.py:1483`, `fio.py:1492`, `backfill_decisoes_pergunta.py:245`. |
| X7 | **CONFERE** — O reaponte já trata `False` como recusa e segue; as perdedoras passam pela porta própria. `fio.py:2328`, `fio.py:2329`, `fio.py:2334`. |
| X8 | **CONFERE** — Fora de cerca, deixar a vetada sem reata e permitir o fechamento decorre da classe (ii); não há colisão identificada nos avais dados. Outras recusas conservam o `ValueError`. `models.py:410`, `models.py:435`, `models.py:438`. |
| X9 | **CONFERE** — O laço registra o resultado de cada reaponte e já pula a pergunta apontada ao pai. `limbo_folgas.py:239`, `limbo_folgas.py:242`, `limbo_folgas.py:245`. |
| X10 | **CONFERE** — A vetada permanece **sem reata**; se o furo não fecha e outra reatou, o rollback desfaz as reatas e o gate é tentado novamente. A trilha proposta precisa ser drenada fora do savepoint. `limbo_folgas.py:278`, `limbo_folgas.py:283`, `limbo_folgas.py:285`, `limbo_folgas.py:289`. |
| X12 | **CONFERE** — A saída de recusa do reaponte serve ao julgamento por item; o descarte B4 tem fundamento na colisão e no índice, preservando os demais itens do laço. `reatar_beco_folga_ao_limbo.py:135`, `fio.py:939`, `fio.py:944`. |
| X13 | **CONFERE** — O fecho aciona S144 e herda o ponto F9 de X1, após as saídas sem escrita. `materializacao.py:2030`, `signals.py:164`, `models.py:3264`. |
| X14 | **CONFERE** — A emissão chama o fecho, cuja cadeia S144 herda X1. `disputa_emissao.py:1298`, `signals.py:164`, `materializacao.py:2167`. |
| X15 | **CONFERE** — A ordem proposta libera a cerca antes do callback; o descarte B9b está apoiado no grupo da ida e na reversão integral já declarados no código. `fio.py:1438`, `fio.py:1449`, `migrar_data_evento_dia_do_turno.py:382`, `migrar_data_evento_dia_do_turno.py:482`. |

**PROCEDE: 3, LINHAS RESPONDIDAS: 29 de 29.**

## 17/11 20:17 · sessao a06993d1296e · CURA-DE-FASE · censo · origem=Code

O censo ficou **incompleto**: confirmei a cadeia da virada e alguns outros sítios, mas não rastreei a origem de todas as datas nem validei todos os candidatos da varredura. Portanto, os totais pedidos não estão verificados.

| Chamador | Origem da data e veredito |
|---|---|
| [censo_fase_12x36.py:413](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:413) → `escala_vigente` | **TEXTO POSSÍVEL.** `linhas` recebe `virada` por `str(virada)` em [censo_fase_12x36.py:224](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:224); o filtro forma `_auto` em [censo_fase_12x36.py:257](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:257), entregue a `_curar` em [censo_fase_12x36.py:268](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:268). |
| [servico_jornada.py:66](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/servico_jornada.py:66) → `vinculo_do_dia` | **TEXTO POSSÍVEL.** Repassa o parâmetro `data` recebido em [servico_jornada.py:15](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/servico_jornada.py:15), inclusive pela cadeia do censo acima. |
| [alimentacao.py:285](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:285) → `vinculo_cobre_a_data` | **TEXTO POSSÍVEL.** Repassa o parâmetro de [alimentacao.py:228](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:228), alcançado pela cadeia anterior; a comparação com `data_inicio` está em [alimentacao.py:214](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:214). |
| [aplicar_col900.py:176](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:176) → `vinculo_do_dia` | **DATA GARANTIDA.** `dias` é construído com `date.fromisoformat` em [aplicar_col900.py:159](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:159) e percorrido em [aplicar_col900.py:172](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:172). |
| [aplicar_col900.py:229](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:229) → `vinculo_do_dia` | **DATA GARANTIDA.** O valor de `it['dia']` passa por `date.fromisoformat` em [aplicar_col900.py:226](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/aplicar_col900.py:226). |
| [diff_col900.py:69](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/diff_col900.py:69) → `vinculo_do_dia` | **DATA GARANTIDA.** `it['dia']` passa por `date.fromisoformat` em [diff_col900.py:66](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/diff_col900.py:66). |
| [alimentacao.py:196](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/calculador/alimentacao.py:196) → `vinculo_do_dia` | **DUVIDOSO.** `_dia` vem dos valores de `resultado.dia_da_jornada` em [alimentacao.py:193](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/calculador/alimentacao.py:193). Falta rastrear quem preenche esse mapa, inclusive no caminho da lavra. [não verificado] |
| [fechamento.py:191](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/fechamento.py:191) → `vinculo_do_dia` | **DUVIDOSO.** `_d` vem da chave do mapa `celulas` em [fechamento.py:189](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/fechamento.py:189). Falta rastrear a construção do mapa em todos os chamadores. [não verificado] |

**TEXTO POSSÍVEL: [não verificado]** — há três sítios confirmados na tabela, contando separadamente o repasse entre funções.

**DUVIDOSO: [não verificado]** — há pelo menos os dois sítios apresentados; faltam os demais candidatos.

A varredura abaixo retornou **119 candidatos fora de testes e 54 em testes**. Ela reconhece imports locais, aliases e chamadas por atributo, mas é uma busca de candidatos: não resolve escopos, sombreamento ou proveniência entre funções. Assim, esses números **não são a contagem final de chamadores nem a contagem validada de testes**. [não verificado] [nao verificado]

```python
import ast
import pathlib
import collections

names = {
    "escala_vigente",
    "vinculo_do_dia",
    "vinculo_cobre_a_data",
    "q_vinculo_cobre_a_data",
}
counts = collections.Counter()

for p in sorted(pathlib.Path("app").rglob("*.py")):
    try:
        t = ast.parse(p.read_bytes())
    except (SyntaxError, ValueError):
        continue

    aliases = {n: n for n in names}
    for x in ast.walk(t):
        if (
            isinstance(x, ast.ImportFrom)
            and (x.module or "").endswith(("servico_jornada", "alimentacao"))
        ):
            for a in x.names:
                if a.name in names:
                    aliases[a.asname or a.name] = a.name

    parent = {
        child: node
        for node in ast.walk(t)
        for child in ast.iter_child_nodes(node)
    }

    for x in ast.walk(t):
        if not isinstance(x, ast.Call):
            continue

        f = x.func
        target = None
        if isinstance(f, ast.Name) and f.id in aliases:
            target = aliases[f.id]
        elif isinstance(f, ast.Attribute) and f.attr in names:
            target = f.attr
        if not target:
            continue

        q = parent.get(x)
        while q and not isinstance(q, (ast.FunctionDef, ast.AsyncFunctionDef)):
            q = parent.get(q)

        test = (
            any(part in ("tests", "test") for part in p.parts)
            or p.name.startswith("test_")
        )
        counts["test" if test else "main"] += 1

        if not test:
            function = q.name if q else "<module>"
            print(f"{p}:{x.lineno} {function} {target} {ast.unparse(x)}")

print("COUNTS", dict(counts))
```

## 17/11 20:17 · sessao 9ce9c6b0f615 · - · - · origem=Ronald (tmux)

Pontos enviados ao agente principal:

- F6/B4: a porta lê `data_evento` gravado, não o valor novo em memória; a proposta de B4 precisa preservar essa leitura literal ([fio.py:1403](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1403), [fio.py:916](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:916)).
- F4: a gravação por `evento` é protegida, mas a consulta anterior de deduplicação não está dentro desse tratamento ([reconciliador.py:489](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:489), [observ.py:105](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/core/observ.py:105)).
- F9: as duas saídas sem escrita precedem a lavra e o save ([models.py:3241](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3241), [models.py:3264](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/models.py:3264)).
- O coletor é proposto; `_ESPERAS_163` é uma referência de forma, já existente ([materializacao.py:1728](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/materializacao.py:1728)).

## 17/11 20:17 · sessao 8f35ea13b72c · - · - · origem=Ronald (tmux)

- **B4 — PROCEDE:** a célula atribui a trilha ao chamado de destino, quando houver, e também manda usar sempre `PerguntaDisputa`; as duas instruções divergem. O código atual registra a retenção no chamado ([reconciliador.py:489](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/reconciliador.py:489)). A célula também diz que a guarda lê `data_evento` da instância, mas ela lê o dia do turno da instância e o dia gravado ([fio.py:1403](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:1403)). O risco de colisão ao datar a principal com perdedora viva está documentado em [fio.py:939](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/services/fio.py:939).

- **B9a — PROCEDE:** o código confirma o aval anterior por grupo ([backfill_decisoes_pergunta.py:245](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/backfill_decisoes_pergunta.py:245)), mas o aval transcrito das 18:45 inclui backfill e determina julgamento por item, com os demais seguindo. A prevalência do aval anterior, afirmada no descarte, não está demonstrada pelo dado fornecido [não verificado].

- **B9b — PROCEDE:** o código confirma a ida por grupo e a reversão tudo ou nada ([migrar_data_evento_dia_do_turno.py:380](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:380), [migrar_data_evento_dia_do_turno.py:482](/home/ronald/codex_ws/afda61fbd665ab31c8f68e53beb94b98187efe8c/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:482)). A compatibilidade dessas unidades com o comando posterior de julgamento por item não está resolvida no dado fornecido [não verificado].

## 17/11 20:17 · sessao 48db4edd951f · PORTA-DO-EXPORT-SEM-BECO · construtor · origem=Code

A implementação e os testes Ra–Rg estão escritos na cópia, mas a entrega **ainda não está certificada**: não executei os testes nem os mutantes, pois eles usam banco; seus resultados e a conformidade completa com o aval permanecem **[não verificado]**. A análise sintática dos módulos de produção e do novo arquivo de testes passou, assim como `git diff --check` **[verificado localmente]**. [nao verificado]

Arquivos alterados:

- [porta_export.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1302): predicado de tentativa morta sem escrita, leitura do estado, espera opcional e textos. `conferir` conserva a lista técnica no estado divergente ([porta_export.py:811](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:811)).
- [selo_leitores_no_mesmo_numero.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:145): espera e encerramento pelo CAS para `--tentativa`.
- [views.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/views.py:315), [urls.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/urls.py:18) e [_estado_da_porta.html](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/templates/folha/_estado_da_porta.html:1): rota, inclusão no preview e bloco com recarga em conferência.
- [test_porta_estado_na_tela.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_estado_na_tela.py:33): casos Ra–Rg. A capacidade de cada caso matar seu mutante permanece **[não verificado]**.
- [test_porta_sem_beco.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_sem_beco.py:107) e [test_porta_sem_beco_item2.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_sem_beco_item2.py:226): inversões autorizadas para leitura sem escrita e persistência pelo vigia.
- [test_selo_leitores_no_mesmo_numero.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:135): mock aceita argumentos nomeados.
- [test_porta_do_export.py](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_do_export.py:53): atualização das três asserções de texto autorizadas.

As três trocas foram `NAO FOI MEDIDA HOJE` → `ainda nao foram conferidos hoje`, preservando as mensagens de falha:

- TXT sem medição: [linha 53](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_do_export.py:53).
- Carimbo de outra competência: [linha 529](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_do_export.py:529).
- Carimbo de ontem: [linha 536](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_porta_do_export.py:536).

A busca pelos três textos antigos nos testes encontrou outras ocorrências em comentários, não em asserções ([test_integracao.py:158](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_integracao.py:158), [test_sm1_monster_export.py:122](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/tests/test_sm1_monster_export.py:122), [test_cauda_trilha_do_cartao.py:107](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/relatorios/tests/test_cauda_trilha_do_cartao.py:107)). [nao verificado]

Para execução posterior em ambiente de testes autorizado, o comando dos RED é: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test folha.tests.test_porta_estado_na_tela
```

Os comandos abaixo aplicam os mutantes **em memória** e executam os casos correspondentes; não foram executados **[não verificado]**.

Mutante de leitura que chama `vigiar`, esperado falhar em Rb e Rc:

```bash
PYTHONDONTWRITEBYTECODE=1 python - <<'PY'
import os
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings.saas')
import django
django.setup()
from folha import porta_export as pe
from django.core.management import execute_from_command_line
original = pe.estado_da_porta
def mutante(empresa, mes, ano):
    pe.vigiar(empresa, mes, ano, pe._linha_do_pedido(empresa, mes, ano))
    return original(empresa, mes, ano)
pe.estado_da_porta = mutante
execute_from_command_line(['manage.py', 'test',
    'folha.tests.test_porta_estado_na_tela.EstadoNaTelaTest.test_Rb_curso_morto_nao_grava',
    'folha.tests.test_porta_estado_na_tela.EstadoNaTelaTest.test_Rc_pedido_vencido_nao_grava'])
PY
```

Mutante de trava sem espera, esperado falhar em Rd:

```bash
PYTHONDONTWRITEBYTECODE=1 python - <<'PY'
import os
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings.saas')
import django
django.setup()
from folha import porta_export as pe
from django.core.management import execute_from_command_line
original = pe.trava_da_medicao
def mutante(empresa, mes, ano, espera_ate=None):
    return original(empresa, mes, ano)
pe.trava_da_medicao = mutante
execute_from_command_line(['manage.py', 'test',
    'folha.tests.test_porta_estado_na_tela.EsperaDaTentativaTest.test_Rd_espera_ate_liberar_e_mede'])
PY
```

Mapa dos campos no caminho alterado:

| Campo | Escreve | Lê |
|---|---|---|
| `detalhe['estado']` | `pedir`, `_cunhar` e `_mudar_estado` ([porta_export.py:1092](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1092), [porta_export.py:1130](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1130), [porta_export.py:1035](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1035)) | `tentativa_morta` e `estado_da_porta` ([porta_export.py:1302](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1302), [porta_export.py:1345](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1345)) |
| `pedido_em` | `pedir` ([porta_export.py:1103](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1103)) | `tentativa_morta` e comando ([porta_export.py:1313](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1313), [comando:153](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:153)) |
| `concluido_em` | `_mudar_estado`, pelo nome do estado ([porta_export.py:1063](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1063)) | `_campos_do_texto` ([porta_export.py:1392](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1392)) |
| `n` | `resultado_de` fornece; `concluir` grava ([porta_export.py:1224](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1224), [porta_export.py:1265](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1265)) | `_campos_do_texto` ([porta_export.py:1393](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1393)) |
| `backend_pid` | `_cunhar` e `assumir` ([porta_export.py:1147](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1147), [porta_export.py:1213](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1213)) | `tentativa_morta` e `medicao_viva` ([porta_export.py:1307](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1307), [porta_export.py:1003](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1003)) |
| Carimbo | `lavrar`, chamado por `concluir` ([porta_export.py:768](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:768), [porta_export.py:1262](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1262)) | `estado_da_porta` e `conferir` ([porta_export.py:1362](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:1362), [porta_export.py:825](/home/ronald/wt-codex-porta-do-export-sem-beco-201235/app/folha/porta_export.py:825)) |

LEI-AKITA: origem=folha/porta_export.py; testemunha=estado_da_porta; RED=Ra–Rg **[escritos, execução não verificada]**; quem-mais-le=censo do item 0 **[conferência completa não verificada]**; juizes novos=0 **[não verificado]**; padrao=job assincrono com estado visivel + escritor unico **[não verificado]**.

## 17/11 20:17 · sessao 742aae255a9a · ESCALA-NAO-DESCREVE · censo · origem=Code

**O censo não está fechado para liberar o construtor:** há quatro sítios adicionais que dependem do formato agrupado de `lista()` e precisam ser preservados ou adaptados; há também um segundo escritor da mesma chave na rotina de sombra. O número de sítios adicionais que **efetivamente mudarão de comportamento** depende da implementação e está **[não verificado]**. Os quatro são impactos estáticos identificados, não quatro mudanças de comportamento comprovadas. ([cadastro_realidade.py:1044](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1044), [vigia_fio.py:194](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/vigia_fio.py:194), [fechar_avisos_regua_na_lista.py:28](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/management/commands/fechar_avisos_regua_na_lista.py:28), [fechar_chamados_vinculo_na_lista.py:36](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:36), [sombra.sh:157](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/bin/sombra.sh:157))

A leitura foi feita na cópia fornecida, sem criar, editar ou apagar arquivos. Rodei `python3` com `ast` sobre os arquivos Python de `app/`, procurando chamadas e imports, inclusive aliases, imports locais e relativos; complementei com buscas textuais no repositório. A associação estática de nomes não prova chamadas dinâmicas nem resolução completa de escopo. O hash efetivo da cópia é **[não verificado]**: o diretório não apresenta metadados Git acessíveis. [nao verificado]

**A — Escrita e armazenamento**

- `anomalias_da_frota()` constrói o retrato; `lavrar()` chama `gravar_lavra(d)`. O comando `esmeril_espelho --apply` chama `lavrar()`, e o cron o agenda. ([esmeril_espelho.py:206](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:206), [esmeril_espelho.py:296](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:296), [comando esmeril_espelho.py:16](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/esmeril_espelho.py:16), [crons.py:640](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/config/crons.py:640))
- A autoridade é `inteligencia.MetricaSnapshot.detalhe`, JSON identificado por `chave='esmeril_espelho'`, `escopo='frota'`, `escopo_id=None` e `data_ref=hoje`; `ler_lavra()` escolhe a `data_ref` mais recente. `lavrar()` também escreve o espelho JSON no caminho configurado, por padrão `logs/esmeril_espelho.json`. ([esmeril_espelho.py:242](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:242), [esmeril_espelho.py:303](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:303), [esmeril_espelho.py:317](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:317), [esmeril_espelho.py:334](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:334), [models.py:81](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/inteligencia/models.py:81))
- **Outro escritor da mesma chave:** `bin/sombra.sh` copia o `detalhe` de produção e faz `update_or_create` diretamente na sombra, além de gravar outra chave de marcador. Portanto, `gravar_lavra` é o escritor do fluxo normal encontrado, mas não o único código que escreve essa chave em todos os ambientes. ([sombra.sh:136](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/bin/sombra.sh:136))

**B — Leitores**

| Leitor | O que lê |
|---|---|
| Cadastro x Realidade | `lista()` lê `por_colab` e `destino`; o template exibe a frase, e `pdf_bytes()` recebe a saída de `lista()` e a imprime. ([cadastro_realidade.py:872](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:872), [template:29](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/templates/escala/cadastro_x_realidade.html:29), [cadastro_realidade.py:1083](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1083)) |
| Pautas do esmeril | `sincronizar()` lê a lavra; `alvo_da_lavra()` usa `por_colab`. A Pauta semanal também chama `linhas_para_a_pauta()`, que usa `por_assinatura`. ([pautas_do_esmeril.py:27](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/pautas_do_esmeril.py:27), [pautas_do_esmeril.py:52](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/pautas_do_esmeril.py:52), [esmeril_espelho.py:356](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:356), [fase_12x36.py:241](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/fase_12x36.py:241)) |
| Copiloto/mensageria | Sem termo, lê a lavra e devolve somente os campos de resumo e `por_assinatura`; com termo, calcula `anomalias_do_colab()` na hora. Essa API não devolve `destino` nem `por_colab` nesse caminho. ([views_mensageria.py:1786](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/api/views_mensageria.py:1786)) |
| Placar | Chama o comando de leitura e extrai o contador, sem ler diretamente os dois mapas. ([placar_code.sh:389](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/bin/placar_code.sh:389), [comando esmeril_espelho.py:17](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/esmeril_espelho.py:17)) |
| Validação de chamados | `sem_marco_por_colab()` lê `por_colab` para rotular o cadastro na fila. ([validacao.py:1129](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/validacao.py:1129), [views_cobrar.py:367](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/views_cobrar.py:367)) |
| Escala automática | `dry_da_frota()` lê `por_colab` e passa as assinaturas às propostas do vínculo. ([escala_auto.py:163](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/escala_auto.py:163)) |
| Sombra | Lê diretamente o snapshot para copiá-lo; a guarda de sombra compara o hash do detalhe com o marcador. ([sombra.sh:138](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/bin/sombra.sh:138), [esmeril_espelho.py:266](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:266)) |

Sobre `horas_folga_trabalhada`: a referência em `config/crons.py` aparece em **:1061**, não em :1072, e descreve uma definição histórica. Na cópia atual, `escala_certa` chama `assinaturas_do_colab(..., so_escala=True)` para a competência do dia, sem ler a lavra; o cálculo usa essa decisão para distribuir as horas. Assim, alterar a função compartilhada de assinaturas exige preservar esse caminho, embora acrescentar campos apenas ao retrato não torne a folha leitora deles. ([crons.py:1058](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/config/crons.py:1058), [escala_certa.py:19](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/escala_certa.py:19), [regras.py:664](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/calculador/regras.py:664))

**C — Chamadores**

- `realocar_vinculo_para_destino`: a ação C7 da lista e o adaptador dos chamados antigos `realocar_para_destino`; há chamadas de teste. A porta determina hoje, recusa destinos cíclicos e chama `executar_vinculo(request, ...)`. ([views.py:463](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:463), [regua_defesa.py:291](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/regua_defesa.py:291), [regua_defesa.py:326](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/regua_defesa.py:326), [test_termometro_camada3.py:100](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/tests/test_termometro_camada3.py:100))
- `propor_escala`: esmeril, `proposta_pdf.coletar`, `escala_auto` e comando `propor_escala_limbo`, além de testes. No esmeril atual, só é chamado quando a pessoa tem alguma assinatura; a confiança está fora do dicionário `proposta` retornado. ([esmeril_espelho.py:213](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:213), [proposta_pdf.py:45](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/proposta_pdf.py:45), [escala_auto.py:119](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/escala_auto.py:119), [propor_escala_limbo.py:37](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/management/commands/propor_escala_limbo.py:37), [detector_proposta.py:153](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/detector_proposta.py:153))

**D — 6x1 e template vigente**

O campo é `ec.tipo_escala.tipo_ciclo`, comparado a `TipoEscala.CICLO_6x1`. O Plano de Folgas chama `ec.defeito_folga_nao_definida()` para seu indicador; esse método contém a comparação. O teste de 6x1 deve olhar o ciclo, e não usar o resultado inteiro do método como sinônimo de 6x1: ele também verifica folga fixa e `FolgaDia`. ([views_plano_folgas.py:83](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views_plano_folgas.py:83), [models.py:1241](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:1241), [models.py:30](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:30))

Para o vínculo vigente hoje, `_vinculos()` filtra com `q_vinculo_cobre_a_data(hoje)`, resolve com `vinculo_do_dia()` e publica `ec.tipo_escala_id` como `template_id`; será necessário também disponibilizar o ciclo para a decisão da linha. O Plano de Folgas atualmente filtra seus vínculos por `ativa=True`, diferença que merece teste no clique. ([cadastro_realidade.py:748](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:748), [views_plano_folgas.py:55](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views_plano_folgas.py:55))

**E — Dependências da lista, PDF, template e rotas**

A view chama `lista()` uma vez, envia o resultado ao HTML ou a `pdf_bytes()`. Hoje o contrato devolve `postos`, com pessoas ordenadas por nome dentro de cada posto; HTML e PDF iteram esse agrupamento. A C7 usa POST, `_is_gestor` e destino de outra lavra, a do termômetro. ([views.py:440](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:440), [views.py:475](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:475), [cadastro_realidade.py:930](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:930), [template:21](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/templates/escala/cadastro_x_realidade.html:21), [cadastro_realidade.py:1083](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1083), [urls.py:14](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/urls.py:14))

Os quatro sítios adicionais identificados que leem `postos` são:

1. `dias_na_lista()`, usado pelo vigia do fio para reconhecer dias e donos. ([cadastro_realidade.py:1030](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1030), [vigia_fio.py:195](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/vigia_fio.py:195))
2. O próprio vigia do fio, que também percorre `postos` diretamente para obter pessoas. ([vigia_fio.py:196](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/vigia_fio.py:196))
3. O comando que identifica avisos C7 para eventual fechamento. ([fechar_avisos_regua_na_lista.py:28](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/management/commands/fechar_avisos_regua_na_lista.py:28))
4. O comando equivalente para chamados C8. ([fechar_chamados_vinculo_na_lista.py:36](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:36))

Remover `postos` sem adaptação faria esses leitores perderem as linhas ou falharem; manter compatibilidade pode evitar mudança funcional, mas sua compatibilidade com a exigência literal de “uma lista só” depende da solução concreta **[não verificado]**. ([cadastro_realidade.py:1044](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1044), [vigia_fio.py:196](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/vigia_fio.py:196))

Há outra premissa a fechar: a janela do esmeril é calculada **por pessoa**, considerando empresa, competências entregues e existência de células. Um único “desde DD/MM” para o total não está necessariamente definido pelo código atual; a data comum aplicável ao dado de produção é **[não verificado]**. ([esmeril_espelho.py:38](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:38), [esmeril_espelho.py:219](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:219))

**F — Arquivos estimados para a obra**

- [esmeril_espelho.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:79): contagem no laço existente, publicação dos mapas e preservação dos demais chamadores de assinaturas.
- [cadastro_realidade.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:717): dados e ações da lista, adaptação de `dias_na_lista` e PDF; o PDF fica neste mesmo arquivo.
- [views.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:440), [urls.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/urls.py:14) e [cadastro_x_realidade.html](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/templates/escala/cadastro_x_realidade.html:21): rota, autorização e apresentação.
- Potencialmente os três arquivos externos que percorrem `postos`: [vigia_fio.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/services/vigia_fio.py:194), [fechar_avisos_regua_na_lista.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/management/commands/fechar_avisos_regua_na_lista.py:27) e [fechar_chamados_vinculo_na_lista.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:31); a necessidade de editá-los depende do contrato escolhido **[não verificado]**.
- Testes existentes, incluindo [test_cadastro_x_realidade.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/tests/test_cadastro_x_realidade.py:42), [test_termometro_camada3.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/tests/test_termometro_camada3.py:86) e [test_o261_vigia_fio.py](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/chamados/tests/test_o261_vigia_fio.py:651), mais casos necessários para R1–R7 **[estimativa; não verificado até a implementação]**.

O cruzamento de arquivos com as raias B2, F2 P2, c4 e PORTA v6, seu estado atual e eventuais colisões são **[não verificado]**: o dado fornecido identifica as raias, mas não fornece suas listas atuais de arquivos. A medição contra 1267, as quantidades fora da lista e o estado da lavra de produção também são **[não verificado]**.

## 17/11 20:16 · sessao 96041dfececd · CURA-DE-FASE · verificador · origem=Code

**1 — Premissas: CONFERE, com ressalva sobre a escrita.** O censo monta `virada` e `proposta_ancora` como texto; `_curar` passa `x['virada']` a `escala_vigente`, que encaminha a data a `vinculo_do_dia`; no caminho sem geradora, a comparação pode chegar a `date <= str`. Há também comparação com data no caminho com geradora. ([censo_fase_12x36.py:224](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:224), [censo_fase_12x36.py:413](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:413), [servico_jornada.py:63](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/servico_jornada.py:63), [alimentacao.py:214](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:214), [alimentacao.py:282](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:282))

As comparações de `NO_OP` usam texto; manter texto no `post` **fecha o contrato dessa porta**: `julgar_vinculo` converte tanto `data_inicio` quanto `data_ancora` com `datetime.date.fromisoformat`. A execução de vínculo ocorre depois da chamada que falhou, mas a reversão em arquivo é escrita **antes** dela. O anexo relata zero vínculos mudados nessa corrida; a confirmação independente em produção é **[não verificado]**. ([censo_fase_12x36.py:397](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:397), [censo_fase_12x36.py:414](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:414), [vinculo.py:52](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:52))

**2 — Colisão com aval vigente: não demonstrada [não verificado].** O corte registrado reserva a rodada de produção ao Ronald, restrita a `--mes 11 --ano 2026`; o teste em banco de teste não é essa rodada. Há teto documentado de dois construtores, e o anexo concede exceção nominal a esta cura; a ocupação atual e o cruzamento real de arquivos com raias em voo são **[não verificado]**. A abrangência do item 2 ainda depende do censo: não é possível garantir agora que todos os sítios eventualmente encontrados caibam no arquivo nomeado ou fora de zonas invioláveis. ([CORTES.md:537](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:537), [CLAUDE.md:141](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:141), [CLAUDE.md:291](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:291))

**3 — R1: CONFERE como teste construível; resultado [não verificado].** `datetime.date.fromisoformat`, já usada na porta, converte a data ISO sem alterar `x` nem o `post`. Há fixtures de teste com empresa, posto, usuário, vínculo e batidas sintéticas; uma linha no formato do censo pode ser montada sem copiar dado de produção. O teste precisa demonstrar a escrita e a saída `desde`, já que a porta pode recusar e `_curar` captura a exceção. Nenhum RED foi executado nesta revisão. ([vinculo.py:54](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:54), [test_fase_12x36.py:34](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/tests/test_fase_12x36.py:34), [censo_fase_12x36.py:421](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:421))

**3 — R2: PROCEDE como ressalva à premissa de alcance automático do `NO_OP`.** Repetir literalmente a mesma lista em `_curar` pode testar o ramo pedido, mas só se, após a primeira escrita, a juíza devolver para a virada um vínculo com **ambos** início e âncora iguais aos textos da linha. A célula e sua geradora influenciam essa resposta; converter a data, por si só, não prova a condição. Numa nova execução do comando, a lista é recalculada e filtrada antes de `_curar`: a linha pode deixar de ser automática e nem chegar ao ramo. A reversão JSON é fotografia de vínculos e células, não a fonte da lista `auto`. Portanto, R2 deve fixar e verificar o estado que faz a mesma linha chegar ao `NO_OP`; seu resultado concreto é **[não verificado]**. ([censo_fase_12x36.py:254](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:254), [censo_fase_12x36.py:380](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:380), [censo_fase_12x36.py:413](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:413), [alimentacao.py:269](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/alimentacao.py:269))

**3 — R3: [não verificado].** A classe e o RED por sítio estão definidos no anexo, mas o censo AST pedido ainda não foi feito; não há base para afirmar quantidade, arquivos atingidos ou ausência de colisão. A chamada de origem está confirmada em [censo_fase_12x36.py:413](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/management/commands/censo_fase_12x36.py:413).

**4 — Exportada, dinheiro e `!`: efeito fora do declarado não demonstrado [não verificado].** A porta de vínculo pode regenerar células e acionar reconciliação; a porta de célula declara guarda para exportada e pode acionar recálculo. Logo, não cabe tratar uma futura cura em produção como sem possível efeito sobre dinheiro; o alcance real dos dados e de outros sítios do item 2 é **[não verificado]**. O corte reserva a execução produtiva ao Ronald. ([vinculo.py:315](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:315), [celula.py:112](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/portas/celula.py:112), [celula.py:460](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/portas/celula.py:460), [CORTES.md:537](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:537))

**MEDIR: [não verificado].** Esta cópia não disponibiliza o histórico Git nem o log de produção; não foi possível identificar o commit de origem nem corridas posteriores por `git log -S` e `logs/censo_fase_12x36.log`. O anexo fornece o resultado da corrida das 20:11, mas não o histórico pedido. O hash da cópia também não pôde ser confirmado por Git **[não verificado]**.

**PROCEDE: 1 — R2**, limitado à premissa de que converter a data garante, por si só, que a segunda passagem alcance `NO_OP`. A existência de um teste com a mesma linha e estado adequado continua possível; seu resultado permanece **[não verificado]**. [nao verificado]

## 17/11 20:16 · sessao 4be1158694af · ESCALA-NAO-DESCREVE · verificador · origem=Code

**AVAL-DEVOLVIDO — PROCEDE nos itens 0, 2 e 4 e no RED R4.** Os achados são: leitor de dinheiro atribuído incorretamente à lavra; início único da janela sem definição; ausência de `!` literal para o toque em vínculo; e R4 exigir sempre um vínculo novo quando a porta também pode atualizar um existente. ([esmeril_espelho.py:351](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:351), [esmeril_espelho.py:38](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:38), [CLAUDE.md:265](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:265), [vinculo.py:100](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:100))

A cópia foi lida nos caminhos abaixo; o hash HEAD informado não pôde ser conferido porque a cópia aberta não contém metadados Git. [nao verificado]

### Item 0 — PROCEDE

- **Premissa do leitor: PROCEDE.** `config/crons.py:1072` não é leitor da lavra: inicia texto descritivo de outra ferramenta; a referência a `horas_folga_trabalhada` está em comentário anterior. A própria implementação de `ler_lavra` declara que dinheiro deixou de ler a lavra; portanto o censo não deve registrar o leitor alegado sem prova por AST. ([crons.py:1060](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/config/crons.py:1060), [crons.py:1072](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/config/crons.py:1072), [esmeril_espelho.py:351](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:351))
- **Escritor e chamadas: CONFERE.** `gravar_lavra` grava o snapshot; há chamadas à porta de realocação pela C7 e pelo caminho de chamado antigo, além de testes. O censo pedido por AST ainda deve distinguir produção e testes. ([esmeril_espelho.py:317](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:317), [views.py:463](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:463), [regua_defesa.py:329](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/regua_defesa.py:329))
- **Colisão e condição de abertura: CONFERE como regra; resultado pendente.** Cruzar arquivos e parar em arquivo comum é compatível com a regra de raias; o cruzamento real com as raias citadas não foi fornecido. A regra de um Codex construtor por vez é afirmada no anexo fornecido, mas sua fonte integral e o estado atual das vagas são [nao verificado]. ([CLAUDE.md:309](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:309))

### Item 1 — CONFERE

- **Premissas e armazenamento: CONFERE.** O laço percorre `ata.orfas` por célula, enquanto A5 conta faixas que atingem limiar; somar `len(orfas)` é uma medida distinta. O retrato inteiro é gravado em `MetricaSnapshot.detalhe`, um `JSONField`; os mapas cabem no mesmo documento e na mesma chave de snapshot, sem migration. ([esmeril_espelho.py:115](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:115), [esmeril_espelho.py:139](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:139), [esmeril_espelho.py:324](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:324), [models.py:95](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/inteligencia/models.py:95))
- **Medida e corte: CONFERE.** O cartório filtra batidas apuráveis antes da grade, de modo que a diferença para 1267 precisa ser medida e explicada, não presumida. A agregação serve à ordem pedida por END-L3; o resultado real, a cobertura das atas e a diferença para 1267 são [nao verificado]. ([cartorio.py:1170](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/cartorio.py:1170), [CORTES.md:535](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:535))
- **Dinheiro/exportada: CONFERE quanto ao desenho declarado de projeção.** A lavra é uma projeção gravada em snapshot; a confirmação de que os leitores permanecerão compatíveis depende do censo e da implementação. ([esmeril_espelho.py:317](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:317))

### Item 2 — PROCEDE

- **Premissa e corte: CONFERE.** Hoje a lista une IDs das assinaturas e do cadastro, filtra empresas visíveis e agrupa por posto; o PDF percorre esses grupos. A ordem global exigirá alterar ambos os consumidores, preservando o universo explicitado no aval. O corte diz “para todos”; a interpretação de limitar isso aos que já entram na lista foi explicitada no aval em revisão. ([cadastro_realidade.py:872](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:872), [cadastro_realidade.py:930](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:930), [cadastro_realidade.py:1083](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:1083), [CORTES.md:535](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:535))
- **Literal da janela: PROCEDE.** Não há um único início da janela de frota armazenado hoje: `janela(c)` depende da empresa, entrega e existência de células da pessoa; o retorno da frota não publica esses inícios, e `data_ini` do snapshot é o dia da gravação. Assim, “desde DD/MM” no topo não tem valor único definido quando as janelas diferem. É necessário definir o significado e a apresentação antes de prometer esse rótulo. ([esmeril_espelho.py:38](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:38), [esmeril_espelho.py:219](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:219), [esmeril_espelho.py:229](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:229), [esmeril_espelho.py:326](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:326))
- **Ausência de medida: [nao verificado].** O aval distingue ausência de zero, mas não especifica posição na ordenação nem texto da linha para uma pessoa da lista sem medida; converter ausência em “0 batidas” contrariaria essa distinção. ([esmeril_espelho.py:95](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:95))

### Item 3 — CONFERE

- **Premissas: CONFERE, com correção de estrutura.** O proponente devolve `tipo_escala_id`, `codigo` e `ciclo` dentro de `proposta`; `confianca` fica no objeto externo. O esmeril o chama apenas quando há assinatura A e atualmente transforma o resultado em frase. Preservar frase e acrescentar mapa cabe no JSON existente. ([detector_proposta.py:153](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/detector_proposta.py:153), [esmeril_espelho.py:221](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:221), [esmeril_espelho.py:234](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:234))
- **Corte e alcance: CONFERE.** END-L1 pede botão para proposta; o aval mantém expressamente o universo atual do proponente. Pessoas presentes apenas por assinatura C podem, portanto, ficar sem proposta; quantas são em produção é [nao verificado]. ([CORTES.md:533](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:533), [cadastro_realidade.py:876](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:876))

### Item 4 — PROCEDE

- **Campos e porta: CONFERE.** “Diferente do template vigente” pode comparar `proposta.tipo_escala_id` com `template_id` do vínculo vigente hoje, já lido por `_vinculos`. Para 6x1, o campo é `ec.tipo_escala.tipo_ciclo == TipoEscala.CICLO_6x1`; o Plano chama a verificação de defeito que usa esse campo. C11 e A11 apontam ao Plano; A2/A8 hoje apontam ao vínculo. ([cadastro_realidade.py:748](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:748), [models.py:1241](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:1241), [views_plano_folgas.py:83](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views_plano_folgas.py:83), [cadastro_realidade.py:708](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:708))
- **Após o clique: CONFERE como desenho possível.** Não é preciso atualizar a lavra: a lista já lê o vínculo vigente no request; comparar seu `template_id` atual com o destino ainda lavrado pode retirar o botão após realocação bem-sucedida. A lavra segue diária, com cron previsto às 08:30. A guarda deve também ser conferida no POST para pedido direto ou repetido. ([cadastro_realidade.py:748](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:748), [crons.py:640](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/config/crons.py:640))
- **Permissão e ciclo: CONFERE.** A C7 exige POST e `_is_gestor`, recupera destino da lavra e chama a porta; a recusa de cíclico está em **310–311**, não em 308. ([views.py:440](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:440), [views.py:463](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:463), [regua_defesa.py:310](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/regua_defesa.py:310))
- **`!`: PROCEDE.** O aval declara o clique e o smoke para uma pessoa, mas não contém `!` literal para dado de escala/vínculo. A seção 7b exige `!` para esse dado, inclusive quando não retroativo, e manda interpretar o escopo literalmente. A porta fecha ou absorve vigências e regenera células; não é apenas leitura. Portanto, o toque de produção do item 4 carece de `!` literal verificável. ([CLAUDE.md:265](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:265), [vinculo.py:211](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:211), [vinculo.py:248](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:248), [vinculo.py:315](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:315))
- **Exportada, dinheiro e pouso: [nao verificado] no caso concreto.** A data de hoje limita o início pedido, mas os dados fornecidos não provam o estado da competência nem o efeito monetário concreto. Se houver apply de dinheiro em competência não exportada, a regra exige reversão prévia, prova de exportada intacta e prova de frota; gravado exportado exige `!` e sua porta própria. O pouso após a simulação é compatível com VESPERA; a régua geral de pouso continua aplicável. ([CLAUDE.md:406](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:406), [CLAUDE.md:411](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:411), [CORTES.md:536](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/docs/CORTES.md:536), [CLAUDE.md:320](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/CLAUDE.md:320))

### RED R1–R7

- **R1 — CONFERE.** Três entradas distribuídas em duas células somam três; pessoa medida sem entradas soma zero. A5 não serve como substituto: conta faixas recorrentes, não o total das entradas. A classificação de pessoa sem células como medida ou ausente precisa ser definida pela implementação sem confundir os estados. ([esmeril_espelho.py:95](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:95), [esmeril_espelho.py:125](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:125), [esmeril_espelho.py:139](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:139))
- **R2 — CONFERE**, sujeito à definição da ausência de medida e ao achado da janela do item 2. O mutante por nome deve usar cargas diferentes para poder ser morto. ([cadastro_realidade.py:962](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:962))
- **R3 — CONFERE.** A estrutura e a frase podem coexistir no mesmo JSON; o teste deve ler efetivamente a frase pelo contrato antigo. ([esmeril_espelho.py:229](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/ponto/services/esmeril_espelho.py:229))
- **R4 — PROCEDE.** A porta procura vínculo com `data_inicio=hoje`: se existir, pode atualizá-lo; só cria outro se não existir. O RED deve fixar a pré-condição de inexistência para exigir “vínculo novo”, ou testar separadamente criação e atualização. A retirada do botão pode usar o vínculo relido, como descrito no item 4. ([vinculo.py:100](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:100), [vinculo.py:259](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/colaboradores/services/vinculo.py:259))
- **R5 — CONFERE.** A porta recusa 12x36 e 24x48 e orienta o wizard; o teste deve provar tanto a ausência do botão quanto a recusa do POST direto. ([regua_defesa.py:310](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/regua_defesa.py:310), [models.py:386](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:386))
- **R6 — CONFERE.** O discriminante 6x1 existe no tipo do vínculo; o controle 12x36 impede generalizar a substituição de A2/A8 a todos. ([models.py:1246](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/models.py:1246), [cadastro_realidade.py:717](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/services/cadastro_realidade.py:717))
- **R7 — CONFERE.** A guarda da C7 fornece o padrão verificável de POST e `_is_gestor`; o RED deve conferir ausência de escrita. ([views.py:440](/home/ronald/codex_ws/ba4cb6c3c33403eecacbefeeef01b64c5b441a83/app/escala/views.py:440))

**PROCEDE: 4 — item 0, item 2, item 4 e R4.**

_6716 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
